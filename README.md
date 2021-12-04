% BME 3053C Final Project 
% Biocomp Solutions
% Authors: Emma Willett, Alejandra Bayer, Rohan Lingam, Jean-Pierre Pierantoni
% Course: BME 3053C Computer Applications for BME 
% Term: Fall 2021
% J. Crayton Pruitt Family Department of Biomedical Engineering 
% University of Florida
% Emails: emmawillett@ufl.edu, alejandra.bayer@ufl.edu, rlingam@ufl.edu, jpierantoni@ufl.edu
% December 4, 2021

clc; clear; close all;
fprintf('Thank you for using our image analysis program. \n');
fprintf('In this program you can select \n');
fprintf('a tumor if it shows in the presented image. \n');
fprintf('If you believe a tumor is present, click on the area of interest \n');
fprintf('5 times in different places with varying intensity to better \nvisualize the tumor. If you do not see\n');
fprintf('a tumor in the presented image then click on background space. \n');
fprintf('If a tumor is detected you can choose if you would like \n');
fprintf('learn more about the tumor type that you have detected. \n');
fprintf('\n');
a=input('Type Next to have a random image appear: ','s');
if a(1)=='N'||'n'
    

files = dir('M:\Desktop\BME3053C Comp Apps\Final Project\Images');
files = files(3:end);
s = size(files);
filename = files(randi([1 s(1)],1,1)).name;
image=load(filename);

 I = imread(filename);

Iorig = I;
Iorig = im2gray(Iorig);
figure(1)
imshow(Iorig);

[x,y] = ginput(5);
value = Iorig(uint8(mean(y)),uint8(mean(x)));
Iorig(Iorig>value) = 255;
Iorig(Iorig<value) = 0;

Iorig(1:5,1:10) = 0;
bw2 = bwareaopen(Iorig,500);
bw3 = bwareaopen(bw2,2000);
imgbw = bw2 - bw3;

[~,thresh] = edge(imgbw,'sobel');
fudgeFactor = 0.1;
Iedge = edge(imgbw,'sobel',thresh*fudgeFactor);

se90 = strel('line',5,90);
se0 = strel('line',5,0);
Idilate = imdilate(Iedge,[se90 se0]);

Ifill = imfill(Idilate,'holes');
figure(2)
imshow(Ifill)

figure(3)
imshow(labeloverlay(I,Ifill));
title('Mask Over Original Image');


[rows, cols] = size(I);
if value <30
    fprintf('There is no tumor in this MRI scan\n \n')
elseif x < rows/2
    fprintf('This is a Meningioma tumor. \n \n')
else
    fprintf('This is a Glioma tumor. \n \n')
end

fprintf('Would you like to know more information about this tumor? \n')
d = input('Please input Yes or No : ','s');
if d(1) == 'Y' 
    if x < rows/2
    fprintf('\n Info- Meningioma: \n')
    fprintf('Meningioma is the most common type of primary brain tumor, \n accounting for approximately 30 percent of all brain tumors. \n\n')
    fprintf('These tumors originate in the meninges, which are the outer \n three layers of tissue between the skull and the brain that \n cover and protect the brain just under the skull. \n\n')
    fprintf('Meningiomas grow out of the middle layer of the meninges called \n the arachnoid. They grow slowly and may exist for years before \n being detected. Sometimes doctors will discover a meningioma \n incidentally on a magnetic resonance imaging (MRI) scan of the \n head or spinal cord. \n\n')
    fprintf('For more information, visit \n https://www.hopkinsmedicine.org/health/conditions-and-diseases/meningioma')
    elseif value <30
        fprintf('\n \n There is no tumor in this MRI scan')
    else
        fprintf('\n \n Info- Glioma: \n')
        fprintf('Gliomas can affect all ages, but they are most often seen \n in adults. Gliomas are slightly more likely to occur in men \n than in women, and more common in Caucasians than in African Americans. \n\n')
        fprintf('There are different grades of gliomas, indicating their \n growth potential and aggressiveness. \n\n')
        fprintf('This group of tumors includes glioblastomas. Glioblastoma \n symptoms may be similar to those of other gliomas. \n\n')
        fprintf('For more information, visit \n https://www.hopkinsmedicine.org/health/conditions-and-diseases/gliomas')
    end
else 
    fprintf('Have a good day!')
end
end

