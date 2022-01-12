This is a simple build of the imgui library demo with glfw and opengl2.

Purpose - to make the build of this project easy on any Linux platform. In my case I wanted to build it as well for aarch64, Manjaro Linux for my Raspberry Pi 4 B. 

Problem - all the dependencies, it was not clear what you really need, what you don't, and yes - my CMakeLists is still dirty.
I had to read approx 15 pages / "libs How Tos" / etc to understand what to include in my simple CMakeLists.txt, how do I get and install it.
Building several of the libraries by myself didn't even help...
Reason - I'm total newbie with IMGUI. So I want to spare for you the suffering :).

----------------------------
Solution:

Prerequisites - to have installed GCC and Cmake, potentially whatever development packages you need and work with.

For the imgui demo: First install these packages:
sudo apt-get install xorg-dev
sudo apt-get install libglfw3-dev
sudo apt-get install libglu1-mesa-dev
sudo apt-get install freeglut3-dev

Then download from github the glfw, I use the Tag 3.3.2 from here:
https://github.com/glfw/glfw/releases/tag/3.3.2
 - unzip, run in terminal in the root directory:
 - $ cmake .
 - $ make 
 - $ sudo make install

Finally you have to build the simple "glad" library provided in this package. From terminal navigate to "imgui_aarch64_glfw_openGL2_experiment/glad/".
There run 
 - $ cmake .
 - $ make 

Now navigate to "imgui_aarch64_glfw_openGL2_experiment/imgui/examples/example_glfw_opengl2/".
Again call "cmake ." and "make".

Change the permissions of the resulting file e.g. with 
$ chmod u+x example_glfw_opengl2_cmake

Execute with $ ./example_glfw_opengl2_cmake

To run in background, releasing the console add an & at the end like this:
$ ./example_glfw_opengl2_cmake&

----------------------------
I have tested it on 3 platforms:
    - one Ubuntu; 
    - on a Linux Mint; 
    - built and executed it on the Raspberry Pi 4 B with Manjaro Linux
    
I hope you will enjoy this one :).

Now you have some basics to start with RPI4 linux based gui applications.
