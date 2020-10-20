# pyUSID-process-fitting
A tutorial on how to create a custom fitting class using pyUSID.Process to model spectroscopic data

### Note on Pycroscopy and pyUSID

Pycroscopy / pyUSID is a python module developed by scientists at Oakridge National Lab (ORNL) and the Big-Data Spectroscopy community. pyUSID has been developed for scpectrocipic scientific analysis specifically working with large HDF5 files and is an excellent opensource tool for performing data analysis and modelling on large complex spectroscopic datasets. During myt PhD research, I went to ORNL and interacted with the developpers of pyUSID and got inspired to use it in my research and built a custom fitting function for studying relaxation phenomena that leverages the powerful method called pyUSID.Process. In an effort to help others scientists in the spectroscopic community, I created this tutorial to show how to implement the pyUSID.Process method and build your own custom model class using it. 

### Mapping a Stretched Exponential Relaxation Model to 5-Dim Spectroscopic Piezoforce Microscopy (PFM) Data

Author: Nick Mostovych (from my PhD work)

**Objective:**

Analyze 5-dimentional spectroscopic PFM Data, which I obtained doing 10+hour/scan PFM studies of a novel piezoelectric material system called BNKT-BMT. I want to model the mechanical relaxation behavior as a function of position to see if there is spatial variation across the 2D map scanned. I want to see if there are "fast" and "slow" relaxing regions and how big they are.  

**Background:**

The research objective was to optimze the properties of a piezoelectric material system I developed called (1-x)Bi1/2(Na0.78K0.22)1/2TiO3  ̶  xBiMg1/2Ti1/2O3 (BNKT-BMT). Piezoelectrics are a class of materials that have excellent electromechanical coupling properties. They convert electrical energy to mechanical energy (or vice-versa). We use these materials in many applications ranging from the accelerometers in our cellphones to the fuel injectors in our cars. The doping level (amount of BMT for BNKT-BMT) determines how good these electromechanical properties are. However the science behind what exactly happends with doping is not yet well understood, which limits our ability to optimize these materials fully. The macroscopic properties as a function of doping are well documented. But, what is happening on an atomic scall is not yet well understood. PFM is a scanning probe spectorscopy measurment that can give us localized information about the electomechanical properties of a piezoelectric material down to the nanometer scale. I want to see what happends at various doping levels because just thre right amount of fast and slow regions on the microscopic scale, translates to an optimzed macroscopic performance on the macroscopic scale. I am attemping to quantify spatial variation of relaxation beat just the right size regions The experiments performed which resulted in theses datasets 

**What is the Data:**

Large ~ 20 GB datasets organized in a HDF5 file (Hierarchical Data Format 5). The raw data is the amplitude and phase responses of an electrically conducting nano-cantilever raster scanning (interacting) with 2D surface. The cantilever is stimulated by a chirp exitation pulse at over a frequency band. A DC drive waveform is superimposed ontop to apply different voltage levels. Measurements of the amplitude response are taken both in the "on state" (DC bias on) and "off state" (DC bias off). To study the relaxation data, I am taking many successive measurements as a function of time for each "off state". X-position, Y-position, frequency, DC bias and time make up the 5 dimentions of the dataset. The amplitude and phase cantilever deflection response is recorded as a complex number for each of these spectroscopic conditions

Spectroscopic Dimensions: X, Y, ω, V, t  = 50 x 50 x 203 x 32 x 64 = Very Large Datasets

position:  x-y grid (50x50) = 2500
frequency: 203 
DC bias:   32
Time:      64

Datasets are flattened to be 2D for simplicity of analysis purposes: dataset (spatial, other spectroscopic)

**What I will do with the Data:**

1. Organize the data to seperate the "off state" raw data from the "on state" I want to work with the "off state"  
2. Find the resonance data for each position, DC bias and time; fitting it to a simple harmonic oscillator (SHO) model and create a new SHO dataset only with the resonance data
3. Map a relaxation model to the SHO dataset and create a relaxation dataset composed of a custom datatype, which is composed of all of the model parameters
4. Check that the model makes sense and see what fast and slow regions look like using the Beta parameter of a stretched exponential for comparison
5. Anylyze the results and draw conclusions about spatial variation of relaxation behavior as a function of BMT doping 
