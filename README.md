# Improved-msfm3d
Improved multi-template rapid marching method
# Improved MSFM3D - Microseismic Travel Time Calculation

## Overview

This project provides an **improved version of the Multi-Stencil Fast Marching Method (MSFM)** implemented in MATLAB and C. It aims to enhance the accuracy of microseismic travel time computations in complex 3D geological velocity models by integrating second-order derivatives and cross-neighbor stencil strategies.

## Key Features

- Calculates accurate 3D travel times from a source point to all other points in a given velocity model.
- Supports both:
  - First-order and second-order derivative schemes
  - Standard and multi-stencil neighbor configurations
- Includes multiple geological scenarios:
  - Layered model
  - Gradient model
  - Cylinder with void area model
  - Sphere with void area model
  - Oil cavern model

## Technical Approach

- **Multi-Stencil Fast Marching**: The MSFM algorithm is based on the Fast Marching Method (FMM), which computes the travel times of seismic waves from a source point to every other point in a 3D domain. The main innovation of this project is the incorporation of multiple stencils and higher-order derivatives to improve the robustness and accuracy of the calculation.
  
- **C and MATLAB Integration**: The project combines C for computational efficiency and MATLAB for ease of use. The C code is compiled into a MEX file and called within MATLAB to execute the core algorithm. This hybrid approach ensures that the algorithm can handle large-scale models efficiently while maintaining flexibility for visualization and analysis within MATLAB.

## Folder Introduction

Each folder contains the same C source files (`msfm3d.c` and `common.c`) and a MATLAB script tailored for the respective model.


Programming language: MATLAB + C (compiled and called through MEX interface)

Core files:

msfm3d.c: Main function file, implementing the multi-template fast march core algorithm

common.c: Common function and data structure definition


## Core Features

- **Enhanced Accuracy**: By incorporating second-order derivatives and cross-neighbors, the MSFM improves the precision of the travel time calculations, especially in complex and irregular velocity models.
- **Flexible Modeling**: This project supports a variety of geological scenarios, each of which has its own specific velocity structure. The models include:
  - **Layered velocity models**: Representing stratified geological formations with different seismic wave velocities at each layer.
  - **Gradient velocity models**: Describing gradual transitions in wave velocities.
  - **Cylinder with void area models**: Featuring cylindrical structures, such as tunnels or caverns, with varying velocities.
  - **Sphere with void area models**: Representing spherical cavities or voids embedded in a 3D matrix.
  - **Oil cavern models**: Specifically designed to simulate oil reservoir environments where seismic wave propagation is crucial for microseismic monitoring.

- **Text Output in the Command Window**:

The code calculates the error between each method’s travel time results  and the reference values . The error is computed using three different metrics: L1 , L2 , and Linf .

   - **L1**:mean absolute error
   - **L2**:mean squared error
   - **Linf**: maximum absolute error


## Applications

This method can be widely applied in **seismology**, **geophysics**, and **geological engineering**, particularly for:
- **Microseismic localization**: Accurately determining the location of seismic events in underground environments.
- **Seismic monitoring**: Analyzing and modeling the propagation of seismic waves through complex geological structures.
- **Reservoir characterization**: For modeling fluid flow and understanding structural behavior in oil and gas reservoirs.


## Function Example

Before using the function in MATLAB, you must first compile the code by running mex msfm3d.c. After compilation, you can execute different models by entering the corresponding scripts in the MATLAB command window, depending on the scenario you want to simulate.

**Instructions**:

- **T1_FMM1**: using first-order derivatives without cross neighbors.

- **T1_MSFM1**: using first-order derivatives with cross neighbors.

- **T1_FMM2**: using second-order derivatives without cross neighbors.

- **T1_MSFM2**: using second-order derivatives with cross neighbors.

Examples of core features of this project are:

```matlab

mex msfm3d.c

SourcePoint = [21; 21; 21];
SpeedImage = ones([41, 41, 41]);
[X, Y, Z] = ndgrid(1:41, 1:41, 1:41);
T1 = sqrt((X - SourcePoint(1)).^2 + (Y - SourcePoint(2)).^2 + (Z - SourcePoint(3)).^2) ./ SpeedImage;

% First-order FMM
T1_FMM1 = msfm3d(SpeedImage, SourcePoint, false, false);

% First-order MSFM
T1_MSFM1 = msfm3d(SpeedImage, SourcePoint, false, true);

% Second-order FMM
T1_FMM2 = msfm3d(SpeedImage, SourcePoint, true, false);

% Second-order MSFM
T1_MSFM2 = msfm3d(SpeedImage, SourcePoint, true, true);

% Compare with ground truth T1
fprintf('Method   L1         L2         Linf\n');
Results = cellfun(@(x) ...
    [mean(abs(T1(:) - x(:))), mean((T1(:) - x(:)).^2), max(abs(T1(:) - x(:)))], ...
    {T1_FMM1, T1_MSFM1, T1_FMM2, T1_MSFM2}, 'UniformOutput', false);
fprintf('FMM1:   %9.5f %9.5f %9.5f\n', Results{1});
fprintf('MSFM1:  %9.5f %9.5f %9.5f\n', Results{2});
fprintf('FMM2:   %9.5f %9.5f %9.5f\n', Results{3});
fprintf('MSFM2:  %9.5f %9.5f %9.5f\n', Results{4});

```

## Acknowledgements

Let me know if you'd like to tailor this more for a thesis submission, a GitHub repository, or add author/contributor info.
