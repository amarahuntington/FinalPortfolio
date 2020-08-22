```python
import scipy.io as sio
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import ipywidgets as widgets
from ipywidgets import interact, interact_manual
cond_labels = ['CTRL', 'ADAPT'] 
contr_labels = [4, 8, 12, 16, 24, 32, 48, 64, 84, 100]
rep_labels = list(np.arange(1, 8))
num_reps = len(rep_labels)

time_labels = list(np.arange(4000))

stim_on_time = 2000
stim_off_time = stim_on_time + 1000

adapt_on_time = 0
adapt_off_time = adapt_on_time + 2000

dat = sio.loadmat('crowder_single_unit.mat')

# Compute the total number of data points per neuron; we need to know how
#   many rows our DataFrame needs to have.
len_data = [(x * y * z) for x, y, z in [dat['SaveForAaron_May11_2020'][0][0][1].shape]][0]

# Set up vectors to label the columns in the pandas DataFrame
num_tp = dat['SaveForAaron_May11_2020'][0,0][1].shape[0]
time_labels = list(np.arange(num_tp))
times = time_labels * (len_data//len(time_labels))

num_condcontr = dat['SaveForAaron_May11_2020'][0,0][1].shape[2]
num_cond = len(cond_labels)
num_contr = len(contr_labels)

num_reps = dat['SaveForAaron_May11_2020'][0,0][1].shape[1]
rep_labels = list(np.arange(1, num_reps+1))
reps = np.tile(np.repeat(rep_labels, num_tp), num_cond * num_contr)

# Since condition and contrast are actually separate variables, we'll
#  break them out here.
contrs = np.tile(np.repeat(contr_labels, num_tp * num_reps), num_cond)
conditions = np.repeat(cond_labels, num_tp * num_reps * num_contr)

neuron_labels = ['m1_6', 'm3_4', 'm6_11']
num_neurons = len(neuron_labels)


df_list = []

for n in range(num_neurons):
    neurons = np.repeat(neuron_labels[n], len_data)
    df_tmp = pd.DataFrame(zip(neurons, times, reps, contrs, conditions,
                              dat['SaveForAaron_May11_2020'][0][n][1].reshape(-1, order='F')[None][0]), 
                      columns = ['neuron', 'time', 'repetition', 'contrast', 'condition', 'spike']
                     )
    df_list.append(df_tmp)
    
df = pd.concat(df_list)

# Clear things from memory so CoCalc doesn't run out
del df_tmp
del dat


hist_bin_width = 50 
time_bins = np.arange(0, max(time_labels), hist_bin_width)
```


```python
@interact
def raster_i(Neuron=neuron_labels):
    fig = plt.figure(figsize=[8,18])
    

    neu_dat = df[(df['neuron'] == neuron)]

    subplot_counter = 1 # used to track subplots
    for contr in contr_labels:
        tmp_dat = neu_dat[(neu_dat['contrast'] == contr)] # this further speeds execution by a factor of 3

        for cond in cond_labels:
            ax = fig.add_subplot(num_contr, num_cond, subplot_counter)
            for rep in rep_labels:
            
            # insert code that will select the times corresponding to the rows of 
            #  tmp_dat that match the current condition and rep, and also contain spikes
                spike_times = tmp_dat[(tmp_dat['condition'] == cond) &(tmp_dat['repetition'] == rep) &(tmp_dat['spike'] == 1)]['time']
            
                        # change the line below to plot vertical lines at the times of each spike
                plt.vlines(spike_times, rep-1, rep,color='black', linewidth=.75 )

        # Show adaptation grating at 50% contrast
            if cond == 'ADAPT':
                plt.axvspan(0, stim_on_time-1, alpha=0.5, color='grey')
                                                              
        # shading indicates stimulus on period
            plt.axvspan(stim_on_time, stim_off_time, alpha=contr/100, color='g')
        

            plt.xlim([0, max(time_labels)+1])
            plt.ylim([0, len(rep_labels)])
            plt.title(cond + ' ' + str(contr) + '% contrast')
            plt.xlabel('Time (ms)')
            plt.ylabel('Repetition')
            plt.yticks([x + 0.5 for x in range(num_reps)], [str(x + 1) for x in range(num_reps)], size=8)
            plt.tight_layout()
            subplot_counter += 1
        
    plt.show()
```




    interactive(children=(Dropdown(description='Neuron', options=('m1_6', 'm3_4', 'm6_11'), value='m1_6'), Output(…




```python
@interact
def psth_i(Neuron=neuron_labels):
    fig = plt.figure(figsize=[8,18])
    

    neu_dat = df[(df['neuron'] == neuron)]

    subplot_counter = 1 # used to track subplots
    for contr in contr_labels:
        tmp_dat = neu_dat[(neu_dat['contrast'] == contr)] 
        for cond in cond_labels:
            ax = fig.add_subplot(len(contr_labels), len(cond_labels), subplot_counter)
            plt.axvspan(stim_on_time, stim_off_time, alpha=contr/200, color='g') 
            spike_times = tmp_dat[(tmp_dat['condition'] == cond) & (tmp_dat['spike'] == 1)]['time']
            nOut,bins = np.histogram(spike_times, bins= time_bins)
            plt.bar(bins[:-1], nOut/(num_reps * hist_bin_width), width=hist_bin_width, color='b')
            plt.xlim([0, max(time_labels)+1])
            plt.ylim([0, 0.05])
            plt.ylabel('Spike Probability',style='italic')
            plt.title(cond + ' ' + str(contr) + '% contrast')

        # Show adaptation grating at 50% contrast
            if cond == 'ADAPT':
                plt.axvspan(0, stim_on_time-1, alpha=0.5, color='grey')
                                                          
        
            plt.tight_layout()
            subplot_counter += 1
        
    plt.show()
```




    interactive(children=(Dropdown(description='Neuron', options=('m1_6', 'm3_4', 'm6_11'), value='m1_6'), Output(…


