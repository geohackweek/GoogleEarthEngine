FROM continuumio/anaconda3
RUN apt-get install -y tar git curl nano wget dialog net-tools build-essential
RUN apt-get -y install pyqt4-dev-tools
RUN mkdir /data
RUN conda install matplotlib
RUN conda install pandas
RUN conda install numpy
RUN conda install xarray
RUN conda install dask
RUN conda install netcdf4
RUN conda install jupyter
RUN mkdir /notebooks
COPY /data/ /data
