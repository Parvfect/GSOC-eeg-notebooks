# GSOC-eeg-notebooks
Final Submission for GSOC

## Summary

I worked for EEG Notebooks in the GSOC period, with the main project of refactoring the code of the Experiement Class Library and making it more generic ([https://summerofcode.withgoogle.com/programs/2022/projects/2baQ46dm](https://summerofcode.withgoogle.com/programs/2022/projects/2baQ46dm)). I was able to finish the main project and get the Pull Request Merged just a few weeks prior to the timeline, and therefore was able to pick up a secondary project of adding pipelines for the cli Analysis, which is an open PR about to be merged. As discussed with the mentor John Griffiths, further work with the repository involves creating a default real time visualizer for EEG Notebooks and is described in detail below.

## Project 1 - Refactoring the Experiment Class Library

PR link - https://github.com/NeuroTechX/eeg-notebooks/pull/184

In its initial state, the Experiment Class Library for EEG Notebooks had a lot of repeated code that was non-generic and had a lot of scope for improvement. The main goal with the project was to create a class like structure such that each Experiment derived from a Base Experiment Class that does most of the heavy lifting as per, 

```python
class BaseExperiment():

	def __init__(self, n_trials, ...):
		self.n_trials = n_trials
	
	def load_stimulus():
		""" Needs to be overwritten in derived class """
		return NotImplementedError
	
	def run_experiment():
		...

class VisualN170(BaseExperiment):

	def __init__(self,..):
		super.__init__(..)

	def load_stimulus():
		""" Overriding function to the base class """
	...

experimentObject = VisualN170(..)
experimentObject.run_experiment()
```

After implementing such a structure, more than 500 lines of code from the specific Experiments could be discarded. It also allowed the user to run the experiment as follows, 

```python
from eegnb.experiments import VisualN170
expt = VisualN170()
expt.run_experiment()
```

The code was then tested in different situtations and once we were confident that it had no issues, it was merged to the dev branch.

## Project 2 - Adding Pipelines for cli Analysis

PR link - https://github.com/NeuroTechX/eeg-notebooks/pull/202

EEG Notebooks provides some Jupyter Notebooks to analyse the ERP’s obtained through conducting the Experiments using an EEG. However, they force the user to get his hands dirty with the code and work with it in order to use the analysis functions. It is not possible for a non-developer to easily use the analysis side of EEG Notebooks. In this feature, I attempted to implement an automated analysis function that works with the cli and lets the user conduct analysis without writing any code. 

This involved a few major things:

1. Readymade functions that do the analysis that can be called upon
2. Code that writes the results generated into a pdf
3. Code that autosaves the pdf into the appropriate folder location
4. Interfacing the automation with the cli

After this work was done, I thought it would be a really interesting idea to interface the analysis with a live recording session, that is, to immidiately automatically generate the analysis report for a recording session that is conducted. This would allow the user to see concrete results the minute his recording is done. This was then implemented by interfacing with the cli and debugging a lot of errors due to the large number of files involved and was finally implemented. 

The PR is open and is conducive to be merged soon. The remaining work to be done on it is listed in the latest comment of the PR thread.

## Project 3 and Future Work

Project 3 involves implementing a wrapper over the real time visualizers that are implemented in muse-lsl and Brainflow in order to make it extremely easy for a User to look at the real time data coming from his EEG. Since Brainflow’s visualizer is pretty complicated to use and cannot be directly accessed using a few lines of code, this could prove extremely useful for any user to look at a real time stream of his data. However, I did not get enough time to work on it during the GSOC period and attempt to complete it in the future.
