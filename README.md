# Iso-Codes
# Author = Crossy

import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from tkinter.filedialog import askdirectory
import xlsxwriter

# function for getting peak torque
def peaktorque(a):
    pt = max(a['Torque(Newton-Meters)'])
    return pt

# function for selecting right max sets
def Right(a):
    Right_files = []         # ignoring warm-up sets
    for b in a:
        if 'Right' in b:
            Right_files.append(b)
        #elif 'Right 2' in b:
            #Right_files.append(b)
    return Right_files

# function for selecting left max sets
#def Left(a):
    #Left_files = []         # ignore warm-up sets
    #for b in a:
        #if 'Left 1' in b:
            #Left_files.append(b)
        #elif 'Left 2' in b:
            #Left_files.append(b)
    #return Left_files

# function for averaging top 3 torques recorded
def top3avg(a):
    sorted_list = sorted(a, reverse=True)
    avg_peak_torque = np.mean(sorted_list[0:3])
    return avg_peak_torque

# Locate path
path = askdirectory(title='Select Folder for Data')
IKD_files = os.listdir(path)

#Lefts = Left(IKD_files)
Rights = Right(IKD_files)

def Ext (a):
    df1 = pd.read_csv(path + '/' + a, usecols=[1, 2, 3, 4, 5])
    ex1 = df1[df1['End Pnt 0'] == 0]
    #ex1.drop(ex1.loc[ex1['Speed(d/s)'] < 0].index, inplace=True)
    # above line was #'d, gets rid of negative speeds # pandas error but still exit code 0
    ex2 = df1[df1['End Pnt 0'] == 2]
    ex3 = df1[df1['End Pnt 0'] == 4]
    ex4 = df1[df1['End Pnt 0'] == 6]
    ex5 = df1[df1['End Pnt 0'] == 8]
    ext_peak_torques = [peaktorque(ex1), peaktorque(ex2), peaktorque(ex3), peaktorque(ex4), peaktorque(ex5)]
    return ext_peak_torques

def Flx (a):
    df1 = pd.read_csv(path + '/' + a, usecols=[1, 2, 3, 4, 5])
    flx1 = df1[df1['End Pnt 0'] == 1]
    flx2 = df1[df1['End Pnt 0'] == 3]
    flx3 = df1[df1['End Pnt 0'] == 5]
    flx4 = df1[df1['End Pnt 0'] == 7]
    flx5 = df1[df1['End Pnt 0'] == 9]
    flx_peak_torques = [peaktorque(flx1), peaktorque(flx2), peaktorque(flx3), peaktorque(flx4), peaktorque(flx5)]
    return flx_peak_torques

#Left_data_Ext = []
#for excel in Lefts:
    #for i in (Ext(excel)):
        #Left_data_Ext.append(i)

Right_data_Ext = []
for excel in Rights:
    for i in (Ext(excel)):
        Right_data_Ext.append(i)

#Left_data_Flx = []
#for excel in Lefts:
    #for i in (Flx(excel)):
        #Left_data_Flx.append(i)

Right_data_Flx = []
for excel in Rights:
    for i in (Flx(excel)):
        Right_data_Flx.append(i)

#lext_avg_peak_torque = top3avg(Left_data_Ext)
rext_avg_peak_torque = top3avg(Right_data_Ext)
#lflx_avg_peak_torque = top3avg(Left_data_Flx)
rflx_avg_peak_torque = top3avg(Right_data_Flx)

#print(Left_data_Ext)
#print('Left Extension Peak Torque = ' + str(lext_avg_peak_torque))
print(Right_data_Ext)
print('Right Extension Peak Torque = ' + str(rext_avg_peak_torque))
#print(Left_data_Flx)
#print('Left Flexion Peak Torque = ' + str(lflx_avg_peak_torque))
print(Right_data_Flx)
print('Left Flexion Peak Torque = ' + str(rflx_avg_peak_torque))
