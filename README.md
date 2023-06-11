# Donkeycar: a python self driving library


![Build Status](https://github.com/autorope/donkeycar/actions/workflows/python-package-conda.yml/badge.svg?branch=main)
![Lint Status](https://github.com/autorope/donkeycar/actions/workflows/superlinter.yml/badge.svg?branch=main)
![Release](https://img.shields.io/github/v/release/autorope/donkeycar)


[![All Contributors](https://img.shields.io/github/contributors/autorope/donkeycar)](#contributors-)
![Issues](https://img.shields.io/github/issues/autorope/donkeycar)
![Pull Requests](https://img.shields.io/github/issues-pr/autorope/donkeycar?)
![Forks](https://img.shields.io/github/forks/autorope/donkeycar)
![Stars](https://img.shields.io/github/stars/autorope/donkeycar)
![License](https://img.shields.io/github/license/autorope/donkeycar)

![Discord](https://img.shields.io/discord/662098530411741184.svg?logo=discord&colorB=7289DA)

Donkeycar is minimalist and modular self driving library for Python. It is
developed for hobbyists and students with a focus on allowing fast experimentation and easy
community contributions.

#### Quick Links
* [Donkeycar Updates & Examples](http://donkeycar.com)
* [Build instructions and Software documentation](http://docs.donkeycar.com)
* [Discord / Chat](https://discord.gg/PN6kFeA)

![donkeycar](https://github.com/autorope/donkeydocs/blob/master/docs/assets/build_hardware/donkey2.png)

#### Use Donkey if you want to:
* Make an RC car drive its self.
* Compete in self driving races like [DIY Robocars](http://diyrobocars.com)
* Experiment with autopilots, mapping computer vision and neural networks.
* Log sensor data. (images, user inputs, sensor readings)
* Drive your car via a web or game controller or RC controller.
* Leverage community contributed driving data.
* Use existing CAD models for design upgrades.

### Get driving.
After building a Donkey2 you can turn on your car and go to http://localhost:8887 to drive.

### Modify your cars behavior.
The donkey car is controlled by running a sequence of events

```python
#Define a vehicle to take and record pictures 10 times per second.

import time
from donkeycar import Vehicle
from donkeycar.parts.cv import CvCam
from donkeycar.parts.tub_v2 import TubWriter
V = Vehicle()

IMAGE_W = 160
IMAGE_H = 120
IMAGE_DEPTH = 3

#Add a camera part
cam = CvCam(image_w=IMAGE_W, image_h=IMAGE_H, image_d=IMAGE_DEPTH)
V.add(cam, outputs=['image'], threaded=True)

#warmup camera
while cam.run() is None:
    time.sleep(1)

#add tub part to record images
tub = TubWriter(path='./dat', inputs=['image'], types=['image_array'])
V.add(tub, inputs=['image'], outputs=['num_records'])

#start the drive loop at 10 Hz
V.start(rate_hz=10)
```

See [home page](http://donkeycar.com), [docs](http://docs.donkeycar.com)
or join the [Discord server](http://www.donkeycar.com/community.html) to learn more.

# Q&A
Use https://www.waveshare.net/wiki/PiRacer_Pro_AI_Kit for reference if you use PiRacer Pro AI Kit.
### some pitfalls:
- The joystick keys are mis-mapped, pls be aware of this and try to find the correct key mapping.
- do not need to create env in pi when setting the raspberry pi env.
- change the software source for ubuntu and pip, suggest using tsinghua sources.
- when training data, if you encounter cv2 issues like "str object doesn't have property decode", try to downgrade the h5py version to lower version by running `pip install h5py==2.10.0 --force-reinstall`.
- when setting up Linux PC, clone this repo instead of waveshare/donkeycar and checkout master branch to align with Pi.
- when training data, don't use `python manage.py train --tub /vagrant/data --model ./models/mypilot.h5` use `donkey train --tub /vagrant/data --model ./models/mypilot.h5` instead.
- To enable displayer, use `python -m "pidisplay.display_server"`  the systemctl service doesn't work, or try to find the RC and fix it.
### some commands:

```
# copy data between PC and pi
scp -r pi@192.168.0.106:/home/pi/mycar/data .
scp -r models pi@<ip address of pi>:~/mycar/models/

# run with model
python3 manage.py drive --model ~/mycar/models/mypilot.h5

# train data, "/vagrant/data" is supposed to contain a manifest.json file and multiple catalog files and manifest files and images.
donkey train --tub /vagrant/data --model ./models/mypilot.h5
```




