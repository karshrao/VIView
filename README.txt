VIView Guide
Prepared by Sankarsh Rao (srr2949@mit.edu)


Start-Up
=======================
To open the tool, if you do not already have MATLAB open, double click VIView.mlapp. If this does not work, have MATLAB open and right click VIView.mlapp in the directory window and click "Run".



Default Values
=======================
You will notice that the Pulse Voltage Data field has already been populated with a sample data file. This data file is an experimental voltage chirp from an APG experiment provided in Figure 3 in the paper. Other sample data files are provided in the Data folder.

You will also notice that the scaling parameters in the bottom of the GUI are also filled in with reasonable values for the simulation.

Checking the "Sample Gaussian" overrides the data input file and uses the Gaussian function used in Section 4 of the paper as the voltage input.



Best-Practices
=======================
Some things to note:

* Always start with a wire element -- it is not physical to have no wire/cable between the power supply and the load, so this was not accounted for

* Always end with a wire element -- the ground end condition (it can be very small, say 
Wire = [1e-3, 50, 0.6, 10])

* The Sample Gaussian input is great for rapidly seeing how a waveform's reflections will look -- experimental input files may take longer

* Plots at the load location will plot the difference in voltage across the load and the corresponding current

* Depending on the input the model will take a bit of time -- please be patient! If it takes too long (i.e. gets stuck at 2 or 3/5 in the progress bar), please do control + C to end the processing, re-open VIView and please simplify the inputs. There may be too many points, the max time too long, wires too long, etc.

* On the same vein, spark gap inputs WILL take a while so changing the max time input to have a reasonable computational time is necessary (800 ns --> 200 ns)

* All loads and spark gaps are constant RC loads

* Wire inputs should have non-zero number inputs otherwise singularities will occur

* Loads have 0 length -- they are used as BCs between each wire element. As such, wire-wire interfaces have an implied load of [R,C] = [0,0]

* Solution vectors can be extracted by checking the "Extract Solution Vecs" box. This will save I, V, t, and x as a VIViewOut.mat in the VIView directory



Example 1: Matched Load
=======================
Let's start with a simple example: a wire (cable) leading into a matched load (a resistor that matches the impedance exactly) leading to ground.

Please keep the default values and please input the following by using the Element Type drop down menu and entering the appropriate numbers:

Wire = [5, 50, 0.6, 200]
Load = [50, 0]
Wire = [1e-3, 50, 0.6, 10]

The first wire is a typical 5 meter cable from your power supply to the resistor. The load is a 50 ohm resistor with no capacitance. As all systems for the model need to start and end with a wire, a very small wire with the same properties was included at the end, leading to ground.

Now, check the box for Load Plots (to plot the properties at the load), do not change any of the scaling parameters and click Compute.

The RHS of the GUI should populate within 30 seconds. The Voltage vs. Time plot should be a peak at ~7e-8 secs and the rest of the plots should follow suit and the current profile should look very similar to the voltage. Since it is a matched load, the plots should show that there are little-to-no reflections. Clicking Play under the bottom-left graph should play a video that shows this fact.

Now, let's add a probe at x = 0 and x = 2.5 m. Please input 0, 2.5 into the Probe Location field and click Compute again. You will see that top-left, top-right, and bottom-right plots show this change appropriately.



Example 2: Multiple Loads
=========================
Click Clear All to reset the model and GUI.

Now, let's model a system with multiple loads and changing wire characteristics. Using the same voltage input file, please input the following:

Wire = [5, 75, 0.6, 100]
Wire = [2.5, 50, 0.5, 100]
Load = [5000, 2e-11]
Wire = [3, 50, 0.5,100]
Load = [2500, 2e-11]
Wire = [3, 50, 0.6, 10]

The loads in this case are a good representation of an air-reactor, as they have high resistance and low capacitance. However, in this case sparking is not modeled.

Delete the entries in the Probe Location field, click Compute, and once the results pop up please feel free to explore the results as you wish (play the video, look at the waveforms and notice how most of the energy is deposited in the first load).

Next, lets use the same system but instead use the sample Gaussian as an input: (exp(-(t-1.5).^2/0.2)). This is the same sample Gaussian as used in the paper. Please click the Sample Gaussian check-box, and then click Compute.

Now, please look at the plots and note how everything is smoother than in the experimental input (so detail is lost), but the general shape and trends are the exact same. As such, it is important to note that the Sample Gaussian is very good for rapidly seeing results.


Example 3: Spark Gap
=====================
Now, we can model a load that sparks/breaks down.

Click Clear All and uncheck the Sample Gaussian box so that we use the default experimental file.

Now, input the following:

Wire = [5, 50, 0.6, 100]
Spark Gap = [default]
Wire = [1e-3, 50, 0.6, 0.02] 

The 0.02 points in the last wire is to limit time, instead of 10 which will reach the max MATLAB app memory limit.

Please also change the Max Time parameter to 200e-09 to shorten the max time, so that the simulation does not take too long. Spark gaps are more computationally costly to model, so this model might take a while (on the order of minutes). It will hang on 2/5 in the progress bar, but please be patient!

Let's also extract these solution vectors, so please click that box too.

Now, click Compute and again, please be patient as Spark Gaps take a couple of minutes to model!

This should be a good framework for understanding the tool -- please feel free to use it for your own situations. Another great use is to follow along with Case Studies 1-4, and 6.








