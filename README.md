# bme3053c-project-Biocomp-Solutions

%Read image from folder
d='Yourfolder';
f=dir([d '\*.jpg']);
n=numel(f);
idx=randi(n);
im=imread(f(idx).name);
imshow(im)
