# Fiber Photometry Data Analysis

The package was created to process the data from manuscript: 
__Martianova, E., Pageau, A., Pausik, N., Doucet, T., Leblanc, D, Proulx, C.D.__ _Coherent activity at three major lateral hypothalamic neural outputs controls the onset of motivated behavior responses_

___FiberPhotmetryDataAnalysis.ipynb___ notebook has object and functions necessary to process fiber photometry data.
___LHAretro_allTests_manuscript.ipynb___ notebook is an example of use of the ___FiberPhotmetryDataAnalysis.ipynb___.

### Short description

___FiberPhotmetryDataAnalysis.ipynb___ notebook contains 3 class objects (_FiberPhotometryRecording_, _FiberPhotomentryTest_, and _FiberPhotometryExperiment_) and helper functions to analyze and visualize fiber photometry recordings.   

_FiberPhotometryRecording_ object allows to manipulate recordings from one mouse in a specific test. When the object is created, a user sets raw recordings of calcium-independent (_references_) and calcium dependent signals (_signals_), which were measured during one _trial_, in the form of python dictionary, where keys are names of the neural populations (further _outputs_) the signals were recorded. A user can also set _events_ in the form of dictionary, where keys are names of events (e.g. airpuff, consumption, etc.). The _events_ has to be 2D arrays of two columns, if an event has onset and offset, such as consumption, otherwise one column, e.g. for airpuff. A user can set different _measurements_ that were done along with fiber photometry recordings, such as mobility detection, in the form of nested dictionary, where first keys indicate names of the measurements, and second level of keys  -- their 'time' and 'values'. The object also requires the names of the _mouse_, _test_, and _trial_ in format of python strings

When _FiberPhotometryRecording_ object is created, a user can call functions to calculate dF/F signals (_getDFF()_) and peri-event traces (_getPerievents()_) around the _events_ indicated in the object. The created data can be further saved to a Hierarchical Data Format (HDF) file (_saveRecording()_) and loaded from the file (_loadRecording()_). Recordings that have the same _test_ name are saved in the same group of the HDF file and can be accessed using _FiberPhotometryTest_.

_FiberPhotometryTest_ object allows to manipulate the data from one test across all animals, trials, and neural populations the recordings were done. To create an object, a user has to define name of a HDF file and name of a test. The object also requires that HDF file contains attribute information of _mice_, _outputs_, _tests_ that are saved in the file, and recordings that a user considers good in the form of 2D array with columns: _mouse_, _output_, _test_, and _trial_. The functions allow to combine data, create data frames and plots for peri-event mean values per _mouse_, calculate Pearson correlation, peri-event correlation, and cross-correlation.

_FiberPhotometryExperiment_ object allows to manipulate the data across several tests. To create the object, a user has to define the name of the HDF file containing all the data from different tests. The functions allow plot correlation analysis results across tests.
