# Instructions to produce SIMP LHEs

## Download & install MG5_aMC@NLO
- Download from https://launchpad.net/mg5amcnlo (recommend MG5 2.5.x since I haven't tested anything >2.5.3)
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

