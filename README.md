# MuJoCo Course W02 - Projectile with Drag

MuJoCo bootcamp course from [here](https://pab47.github.io/mujoco.html) ([new version](https://pab47.github.io/mujoco2022.html)).

Instead of the template code from the course, this uses a sample code from the current mujoco release because the current version uses C++ rather than C.

Current version is 2.3.1 - this is hardcoded in the CMakeList MuJoCo reference.


### Building
This is a standard CMake project and I normally run it from CLion.

[CMakeLists.txt](CMakeLists.txt) has hard-coded path to MuJoCo - you will need to update it first to your path.

To manually build & run:

    mkdir build
    cd build
    cmake ..
    build -j4

    ./w02

### Content
#### Setting Camera View
Use object `cam` - see from line 152 (before `while (!glfwWindowShouldClose(window)) {`):

        // you can adjust the camera view:
        cam.azimuth = 90;
        cam.elevation = -45;
        cam.distance = 4;
        cam.lookat[0] = 0;
        cam.lookat[1] = 0;
        cam.lookat[2] = 0;

#### Setting Gravity and other options
Use `m->opt` (see line 160):

      m->opt.gravity[2] = -1;
      m->opt.wind[0] = 100;
      m->opt.wind[1] = 100;

#### Showing Reference Frame
Use `opt` object:

    opt.frame = mjFRAME_WORLD;

#### Set initial position/velocity
Use `qpos/qvel` objects on `mjData`.

Qpos has a value for each generalized coordinate (from `m->nq`). In this case it's somehow 7 - I don't understand how or how to interpret it.

Qvel has a velocity value for each degree of freedom (from `m->nv`). Again, in this case it's 6 - but where are they coming from?

This set's the position of the ball to be 2 in `z` direction and its velocity in `z` and `x` direction:

      d->qpos[2] = 0.1;
      d->qvel[2] = 5;
      d->qvel[0] = 2;

#### Apply Drag Force
Finally, it shows how to apply a drag force using `d->qfrc_applied`. See from line 189:

          double v = sqrt(std::pow(d->qvel[0], 2) + std::pow(d->qvel[1], 2) + std::pow(d->qvel[2], 2));
          double c = 1;

          d->qfrc_applied[0] = -c * v * d->qvel[0];
          d->qfrc_applied[1] = -c * v * d->qvel[1];
          d->qfrc_applied[2] = -c * v * d->qvel[2];


