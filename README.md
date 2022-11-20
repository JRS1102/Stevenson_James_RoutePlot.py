# Routeplot program for JCS Module 3


import numpy as np
import os
import os.path
size = 13

#creates a blank grid of 12x12
def blank_grid():
    grid = np.zeros(([size,size]), dtype = int) #creates a grid with 13 rows and 13 columns of with either numbered axis or number 66
    for i in range(size):
        grid[i][0] = int(size - i-1)
    for j in range(size):
        if j!= 0:
            grid[size-1][j] = int(j)
    return grid

#function takes in input from the user who states the route instructions file to load and checks if file is found in current directory
#if the file is found in the current directory then the file pathway is returned to be used later in the program
#if user enters STOP then function breaks out
#if user enters invalid route instructions then while route runs until valid route instructions or STOP entered
def new_route():
    new_route == True
    while new_route:
        new_file = input("Enter the next route instructions file including the .txt extension, or enter STOP to finish: ")
        if new_file == 'STOP':
            print("No route to plot, thank you for using the route plotter")
            break
        elif os.path.exists(new_file) == True:
            print("File found")
            filename = "C:\\Users\\44797\\Python Code\\"+(new_file)
            return filename
        else:
            print('File not found, please enter new route instructions: ')
            
#defines a function which takes in the blank grid and the filename from the user
#the function converts the route instructions txt file into a list, removes \n chars, checks starting position and then plots each co-ordinate on the grid
def plot_route(grid, filename):
        route_list = []
        route_coords = []
            
        with open (filename) as f:
            for line in f:
                route_list.append(line)
            pos1,pos2 = route_list[:2]
            x = int(pos2)
            y = int(pos1)
            x = int(size -x -1)
                
            if x <0 or x>12 or y<1 or y>13:
                print("Error: The starting co-ordinates are outside of the grid.")
            else:
                route_coords = route_list[2:]
                route_coords = [elem.replace('\n','') for elem in route_coords] 
                grid[x][y] = 15
            next_x = x
            next_y = y
            for value in route_coords:
                if value == "N":
                    next_x -= 1
                elif value == "S":
                    next_x += 1
                elif value == "E":
                    next_y += 1
                elif value == "W":
                    next_y -= 1
                grid[next_x][next_y] = 15

#takes in the grid with co-ordinates of route instructions as integer '15' and replaces them with 'X' and removes 0's
def print_grid(grid):
    finalgrid = grid
    outputgrid = str(finalgrid).replace('[[',' ').replace(' [',' ').replace(']]',' ').replace(']','').replace('15',' X').replace('  0','   ')
    print(outputgrid)

#defines a function which takes in the route list and returns a list if there are no negatives else returns print statement where Route003 contains negatives
def coordinate_list():
    coord_list = []
    remainder = []
    printed_list = []
    with open (filename) as f:
        for line in f:
            coord_list.append(line)
            coord_list = [elem.replace('\n','') for elem in coord_list] # removes the \\n char from list
        x_axis,y_axis = coord_list[:2]
        x_axis = int(x_axis)
        y_axis = int(y_axis)
        printed_list.append(x_axis)
        printed_list.append(y_axis)
        remainder = coord_list[2:]
        new_x = x_axis
        new_y = y_axis
        for i in remainder:
            if i == "N":
                new_y += 1
            elif i == "S":
                new_y -= 1
            elif i == "E":
                new_x += 1
            elif i == "W":
                new_x -= 1
            printed_list.append(new_x)
            printed_list.append(new_y)
        if any(n<0 for n in printed_list):
            return print("Drone crashed out of bounds")
        elif any(n>12 for n in printed_list):
            return print("Drone crashed out of bounds")
        else:
            return printed_list

#functions call a blank grid and take input from the user defining the route instruction file then plots the grid and prints\n",
grid = blank_grid()
filename = new_route()
coord_list = plot_route(grid, filename)
print_grid(grid)

#takes in the list of co-ordinates and turns them into a set of tuple x,y co-ordinates and prints them
#try except block prints the co-ordinates if there are no negative co-ordinates then prints co-ordinates as tuples
#if co-ordinate list returns a drone crashing out of bounds then triggers the except block
try:
    list_of_coords = coordinate_list()
    final_coords = list()
    final_split = 2
    for i in range(0,len(list_of_coords),final_split):
        final_coords.append(list_of_coords[i:i+final_split])
    print(final_coords)
except:
    print("Please enter new route instructions file!\n")
    grid = blank_grid()
    filename = new_route()
    coord_list = plot_route(grid, filename)
    print_grid(grid)
