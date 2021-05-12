# Coherent activity at three major lateral hypothalamic neural outputs controls the onset of motivated behavior responses

This repository includes scripts and source data that were used in the following preprint: 
__Martianova, E., Pageau, A., Pausik, N., Doucet, T., Leblanc, D, Proulx, C.D.__ [_Coherent activity at three major lateral hypothalamic neural outputs controls the onset of motivated behavior responses_](https://www.biorxiv.org/content/10.1101/2021.04.28.441785v1)

[___FiberPhotmetryDataAnalysis.ipynb___](./FiberPhotometryDataAnalysis.ipynb) notebook includes functions necessary to process, analyze, and visualize fiber photometry data. Using these functions, jupyter notebooks in the [rawData](./rawData) folder show all the steps of analysis from raw data to the final [source data](./sourceData) and plots used in the figures of the preprint. [sourceData](./sourceData) folder also includes jupyter notebooks showing stastical analysis done for the preprint.

# How to use functions

## Processing data
Using functions from [___FiberPhotmetryDataAnalysis.ipynb___](./FiberPhotometryDataAnalysis.ipynb), you can create _FiberPhotometryRecording_ object containing your recordings and with one line of code calculate dF/F signal and create perievent arrays:

```python
# Create dictionaries with your recordings and events
signals = {'Aneurons': signal_from_A, # The keys are names of neuronal populations, values are vectors of intensity.
           'Bneurons': signal_from_B} # You can add as many recordings as you have from the same animal.

references = {'Aneurons': reference_from_A, # The keys have to match the one in the signals.
              'Bneurons': reference_from_B}
              
time_ = yourTimeVector # All your recordings and time vectors have to be the same length.

events = {'event1': event1array, # You can add as many events as you have. Each event is a 2D array,
          'event2': event2array, # with 2 columns if it has onset and offset (e.g. consumption, immobility bouts),
          'event3': event3array} # with 1 column if it has only start (e.g. air puff, foot shock).
          
measurements = {'measure1': 'time': measure1time,   # The measurements can be for example speed or freezing score.
                          'values': measure1values} # The recording time of the measurements can be not the same as fiber photometry
                'measure2': 'time': measure1time,   # You can add as many measures as you have.
                          'values': measure1values} 
                          
# Create FiberPhotometryRecording object
r = FiberPhotometryRecording(signals,references,time_,events,
                             measurements,'mouseName','testName','trialNumber')
                             
# Calculate dF/F for all recordings
r.getDFF()
# Get a dF/F trace from one neuronal population
r.dFFs['Aneurons']

# Calculate perievent arrays for each neural population and each event
r.getPerievents()
# Get perievent array recorded from neurons A at the offset of event 3
r.Perievents['Aneurons']['event3']['offset']

# Save data to hdf file
r.saveRecording('HDFname.h5') # In the same HDF file, you can save several recordings from different mice, tests, and trials
```

## Analyzing data
Using FiberPhotometryTest object, you can calculate average per mouse perievent trace across all trials

```
# Create FiberPhotometryTe
test = FiberPhotometryTest('HDFname.h5','testName')

# Calculate means
test.getMeans()

# Plot means for neurons A at onset of event 1
test.plotMeans('neuronsA','event1','onset')
```

