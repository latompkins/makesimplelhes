# Instructions to produce SIMP LHEs

## Download & install MG5_aMC@NLO
- Download from https://launchpad.net/mg5amcnlo (recommend MG5 2.5.x since I haven't tested anything >2.5.3; LT UPDATE 6/8/2023 -- testing with MG3.5)
- Installation should just involve unzipping download in desired dir

## SIMP model
- Untar SM_Ap_Rho_Pi_w_FF.tar (should be SM_Ap_Rho_Pi)
- Copy untarred dir (SM_Ap_Rho_Pi) into your `MG5*/models/` dir

## Running MG5

### Importing the model & generating the process
```bash
./MG*/bin/mg5_aMC
MG5_aMC> import model SM_Ap_Rho_Pi
MG5_aMC> generate e- n > e- n ap /z h DQND=0, (ap > rhod pid, rhod > e+ e-)
MG5_aMC> output TEST_SIMP_GEN -nojpeg -f
MG5_aMC> launch
MG5_aMC> hit "enter" to skip various Pythia/MadSpin options
```

### Edit the param card at prompt
You can change relevant mass parameters which are encoded in GeV (!!!) in the following lines:
```bash
  622 1.000000e-01 # MAp (mass of A')
  623 1.740000e+02 # MNul (mass of target nucleus - don't change unless it's not tungsten)
  624 1.000000e-02 # MPiD (mass of dark pion) 
  625 1.000000e-02 # MRhoD (mass of dark rho)
```
See this param card as an example of parameter settings used to generate SIMPs for HPS:
https://github.com/JeffersonLab/hps-mc/blob/master/generators/madgraph5/src/simp/Events/run_01/run_01_HPS_banner.txt#L399-529

### Edit the run card at prompt
Set nevents and rnd seed:
```bash
  10 = nevents ! Number of unweighted events requested
  0   = iseed   ! rnd seed (0=assigned automatically=default))
```
Set collider type and energy. Keep lpp1/2 as 0 for fixed target
```bash
    0        = lpp1    ! beam 1 type
    0        = lpp2    ! beam 2 type
    2.3     = ebeam1  ! beam 1 total energy in GeV (HPS beam energy in 2016 was 2.3 GeV but change this to LDMX)
    174.0     = ebeam2  ! beam 2 total energy in GeV (HPS target nucleus = 174 GeV but change this for LDMX)
```
See this run card as an example of default settings used to generate SIMPs for HPS:
https://github.com/JeffersonLab/hps-mc/blob/master/generators/madgraph5/src/simp/Events/run_01/run_01_HPS_banner.txt#L116-398

### Run! 
Once you are done editing run & param cards, you can hit enter and MG5 should start generating the subprocesses.

### Check output
When MG5 is done, your output will be inside of `TEST_SIMP_GEN/Events/run_01/unweighted_events.lhe.gz`


# Instructions for running at SDF at SLAC

## Login and setup

### Log into SDF
[SDF](https://sdf.slac.stanford.edu/public/doc/#/ "SDF Docs") is a slac computing system which has a high capacity batch system.  You need to use your SLAC Windows account to access.  It is *not* the same as your SLAC unix account. 
``` 
ssh -Y WINUSERNAME@sdf.slac.stanford.edu
```

### Setting up necessary software

Create a software directory, e.g. ```$HOME/sw```, and follow the steps above to download and install MG5. Then follow the steps under heading "SIMP Model" above.

Setup python 3.9:
```
export SOFTWARE_HOME=/sdf/group/ldmx/software
export PYTHONDIR=$SOFTWARE_HOME/Python-3.9.10/install
PATH=$PYTHONDIR/bin:$PATH
LD_LIBRARY_PATH=$PYTHONDIR/lib:$LD_LIBRARY_PATH
```
Now you should be able to follow the steps above for Running MG5.  It may tell you that you need to 

Full set of steps after having untarred MG5:
```
export SOFTWARE_HOME=/sdf/group/ldmx/software
export PYTHONDIR=$SOFTWARE_HOME/Python-3.9.10/install
PATH=$PYTHONDIR/bin:$PATH
LD_LIBRARY_PATH=$PYTHONDIR/lib:$LD_LIBRARY_PATH
cd ~/sw
mkdir test_making_lhe
python -m pip install six --user
cd test_making_lhe
./../MG5_aMC_v3_5_0/bin/mg5_aMC 
MG5_aMC>import model SM_Ap_Rho_Pi
MG5_aMC>convert model /sdf/home/l/laurenat/sw/MG5_aMC_v3_5_0/models/SM_Ap_Rho_Pi
MG5_aMC>import model SM_Ap_Rho_Pi


