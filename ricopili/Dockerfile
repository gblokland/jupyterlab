FROM bruggerk/ricopili:latest

LABEL org.opencontainers.image.source="https://github.com/gblokland/jupyterlab"

WORKDIR /root

#FROM rocker/rstudio:latest
FROM rocker/r-ver:4.0.0
##FROM rocker/rstudio:4.0.0
##FROM rocker/rstudio-stable:3.4.4 #this R is version 3.3.3, several packages need at least 3.5 or 3.6
##FROM rocker/r-base:4.2.2

MAINTAINER Gabriella Blokland

# system libraries of general use
RUN apt-get update && apt-get install -y \
    sudo pandoc pandoc-citeproc pkg-config libnlopt-dev libcairo2-dev libxt-dev libgsl-dev \
    libssl-dev libssh2-1-dev libxml2-dev libfontconfig1-dev openssl libmpfr-dev libcurl4-openssl-dev \
    libzmq3-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev build-essential libicu-dev \
    cmake wget curl vim rsync git tree tk build-essential bash-completion bc gpg \
    libz-dev zlib1g-dev libudunits2-dev libgdal-dev libv8-dev libmagick++-dev \
    liblapack-dev libblas-dev libharfbuzz-dev libfribidi-dev libssl-dev libbz2-dev liblzma-dev libffi-dev 
    #libcurl-gnutls libssl1.0.0 
    #libcurl4-gnutls-dev conflicts with libcurl4-openssl-dev

# install java
RUN apt-get update && apt-get install -y openjdk-8-jdk openjdk-8-jre
RUN R CMD javareconf

# r-java
RUN apt-get install -y r-cran-rjava 

RUN R -e 'install.packages(c("devtools", "remotes", "RcppEigen"), repos = "http://cran.rstudio.com/")'
RUN R -e 'install.packages("tidyverse", dependencies = TRUE, repos = "http://cran.rstudio.com/")'
RUN R -e 'install.packages("units", dependencies = TRUE, repos = "http://cran.rstudio.com/")'
RUN R -e 'install.packages(c("RSQLite","dbplyr","ggplot2"), dependencies = TRUE, repos = "http://cran.rstudio.com/")'

#Get Linux OS flavor:
#RUN cat /etc/os-release
#Check that 'pkg-config' is in your PATH
RUN which pkg-config
#Tell the system where to search for {package}.pc files:
ENV PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/bin/pkgconfig:/usr/lib/pkgconfig:/usr/share/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/share/pkgconfig 
RUN export PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/bin/pkgconfig:/usr/lib/pkgconfig:/usr/share/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/share/pkgconfig 
RUN echo $PKG_CONFIG_PATH

RUN apt-get update && apt-get install -y python3-pip python3-dev && pip3 install --upgrade pip && pip3 install notebook jupyterlab 
RUN pip3 install --upgrade setuptools && pip3 install ez_setup
#RUN sudo chown $(whoami):$(whoami) /.local

#RUN apt-get update && \
    #apt-get install -y --no-install-recommends r-base && \
    #apt-get install -y r-base r-base-core r-recommended r-base-dev && \
    #apt-get install -y --no-install-recommends r-base-core=4.1.2-1ubuntu2 r-base-html=4.1.2-1ubuntu2 r-doc-html=4.1.2-1ubuntu2 r-base-dev=4.1.2-1ubuntu2 && \

#For debugging: if errors occur that complain about not being able to find {package}.pc files, check this list if they are there:
#RUN ls /usr/lib/x86_64-linux-gnu/pkgconfig/
#RUN ls /usr/lib/pkgconfig/
#RUN cp /usr/lib/x86_64-linux-gnu/pkgconfig/*.pc /usr/lib/pkgconfig/
#RUN ls /usr/lib/pkgconfig/
#RUN ls /usr/local/lib/


RUN mkdir -p /ricopili/dependencies/R_packages && \
    chmod -R 755 /ricopili/dependencies/R_packages

ENV PATH=/usr/local/lib:/usr/bin:/usr/local/bin:/ricopili/dependencies/Minimac3:/ricopili/dependencies/bcftools:/ricopili/dependencies/bgzip:/ricopili/dependencies/eagle:/ricopili/dependencies/eigensoft/EIG-6.1.4/bin:/ricopili/dependencies/impute_v2:/ricopili/dependencies/impute_v4:/ricopili/dependencies/latex:/ricopili/dependencies/ldsc:/ricopili/dependencies/liftover:/ricopili/dependencies/metal:/ricopili/dependencies/shapeit:/ricopili/dependencies/shapeit3:/ricopili/dependencies/tabix:$PATH
ENV PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/bin/pkgconfig:/usr/lib/pkgconfig:/usr/share/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/share/pkgconfig 
RUN export PATH=/usr/local/lib:/usr/bin:/usr/local/bin:/ricopili/dependencies/Minimac3:/ricopili/dependencies/bcftools:/ricopili/dependencies/bgzip:/ricopili/dependencies/eagle:/ricopili/dependencies/eigensoft/EIG-6.1.4/bin:/ricopili/dependencies/impute_v2:/ricopili/dependencies/impute_v4:/ricopili/dependencies/latex:/ricopili/dependencies/ldsc:/ricopili/dependencies/liftover:/ricopili/dependencies/metal:/ricopili/dependencies/shapeit:/ricopili/dependencies/shapeit3:/ricopili/dependencies/tabix:$PATH  && \
    export PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/bin/pkgconfig:/usr/lib/pkgconfig:/usr/share/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/share/pkgconfig

#Standard CRAN packages
#RUN Rscript -e 'dir.create(Sys.getenv("R_LIBS_USER"), recursive = TRUE)' && \
 #   Rscript -e '.libPaths(Sys.getenv("R_LIBS_USER"))' && \
  #  Rscript -e "install.packages(c('bigstatsr', 'bigsnpr', 'corrgram', 'corrplot', 'cowplot'), repos='http://cran.rstudio.com/')" && \
   # Rscript -e "install.packages(c('data.table', 'DescTools', 'pbkrtest', 'doBy', 'dplyr'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('extrafont', 'extrafontdb', 'foreign', 'forestplot'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('randomForest', 'RColorBrewer', 'readxl', 'reshape', 'reshape2'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('gdata', 'ggcorrplot', 'ggplot2', 'ggstats', 'gtools'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('gwasrapidd', 'haven', 'igraph', 'jpeg', 'lattice'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('lubridate', 'meta', 'metafor', 'MetaSKAT', 'rpf'), repos='http://cran.rstudio.com/')" && \ 
    #Rscript -e "install.packages(c('OpenMx', 'pheatmap', 'plyr', 'png', 'poolr', 'psych'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('rmarkdown', 'scatterplot3d', 'scales', 'sem', 'semTools', 'stringr'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('sysfonts', 'systemfonts', 'tibble', 'tidyr', 'ukbtools', 'VennDiagram'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('viridis', 'viridisLite', 'vroom', 'writexl', 'WriteXLS', 'xtable'), repos='http://cran.rstudio.com/')" && \
    #Rscript -e "install.packages(c('mvoutlier', 'qqman', 'rsq', 'xlsx', 'xlsxjars'), repos='http://cran.rstudio.com/')"
    
#Bioconductor packages
#RUN Rscript -e 'dir.create(Sys.getenv("R_LIBS_USER"), recursive = TRUE)' && \
 #   Rscript -e '.libPaths(Sys.getenv("R_LIBS_USER"))' && \
  #  Rscript -e 'install.packages("BiocManager", repos="https://cloud.r-project.org")' && \
   # Rscript -e "BiocManager::install('AnnotationDbi')" && \
    #Rscript -e "BiocManager::install('BiocFileCache')" && \
    #Rscript -e "BiocManager::install('biomaRt')" && \
    #Rscript -e "BiocManager::install('GenomicFeatures')"
    #Rscript -e "BiocManager::install('VariantAnnotation')"
    #Rscript -e "BiocManager::install('org.Hs.eg.db')" && \
    #Rscript -e "BiocManager::install('Rgraphviz')" && \
    #Rscript -e "BiocManager::install('RBGL')"

#RUN Rscript -e 'dir.create(Sys.getenv("R_LIBS_USER"), recursive = TRUE)' && \
 #   Rscript -e '.libPaths(Sys.getenv("R_LIBS_USER"))' && \
  #  #Bioconductor packages
   # Rscript -e "BiocManager::install('ASSET')" && \
    #Rscript -e "BiocManager::install('gwascat')" && \
    #Rscript -e "BiocManager::install('snpStats')"
    ##install snpStats first otherwise LAVA install won't work: dependency ‘snpStats’ is not available for package ‘LAVA’
    
#GitHub packages
#RUN Rscript -e 'dir.create(Sys.getenv("R_LIBS_USER"), recursive = TRUE)' && \
 #   Rscript -e '.libPaths(Sys.getenv("R_LIBS_USER"))' && \
  #  #Sys.unsetenv("GITHUB_PAT")
   # #Rscript -e "remotes::install_github('kenhanscombe/ukbtools', 'ramiromagno/gwasrapidd', 'privefl/bigsnpr', 'leeshawn/MetaSKAT', 'ozancinar/poolR'))" && \
    ##ukbtools gwasrapidd bigsnpr MetaSKAT poolR are now on cran so they get installed with standard packages
    #Rscript -e "remotes::install_github('talgalili/d3heatmap')" && \
    #Rscript -e "remotes::install_github("adayim/forestploter')" && \
    #Rscript -e "remotes::install_github('DudbridgeLab/avengeme')" && \
    ##AVENGEME is a package for polygenic scoring analysis
    #Rscript -e "remotes::install_github('GenABEL-Project/GenABEL.data')" && \
    #Rscript -e "remotes::install_github('MathiasHarrer/dmetar')" && \
    #Rscript -e "remotes::install_github('kassambara/easyGgplot2')" && \
    #Rscript -e "remotes::install_github('josefin-werme/LAVA')" && \
    ##setRepositories(ind = 1:6)
    #Rscript -e "remotes::install_github('weizhouUMICH/SAIGE')" && \
    ##SAIGE is an R package with Scalable and Accurate Implementation of Generalized mixed model.
    #Rscript -e "remotes::install_github('merns/postgwas')" && \
    #Rscript -e "remotes::install_github('MRCIEU/TwoSampleMR')" && \
    #Rscript -e "remotes::install_github('GenomicSEM/GenomicSEM')" && \
    #Rscript -e "installed.packages(); .libPaths(); .libPaths( c( .libPaths(), '/ricopili/dependencies/R_packages', '/usr/local/lib/R/site-library') )"

#Additional genetics tools
#RUN curl -Lo /tmp/bedtools-2.30.0.tgz https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools-2.30.0.tar.gz && \
 #   tar zxvf /tmp/bedtools-2.30.0.tgz -C /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/bedtools-2.30.0.tgz 
#RUN cd /tmp/ && git clone https://github.com/vcftools/vcftools.git && \
 #   cd /tmp/vcftools && ./autogen.sh && ./configure && make && make install && \
  #  cp -r /tmp/vcftools /ricopili/dependencies/ && \
   # chmod 755 /ricopili/dependencies/ && \
    #rm -r /tmp/vcftools 
#RUN cd /tmp/ && git clone https://github.com/gkichaev/PAINTOR_V3.0.git && \
 #   cd /tmp/PAINTOR_V3.0 && bash install.sh && \
  #  cp -r /tmp/PAINTOR_V3.0 /ricopili/dependencies/ && \
   # chmod 755 /ricopili/dependencies/ && \
    #rm -r /tmp/PAINTOR_V3.0 
#RUN curl -Lo /tmp/gcta-1.94.1.zip https://yanglab.westlake.edu.cn/software/gcta/bin/gcta-1.94.1-linux-kernel-3-x86_64.zip && \
 #   unzip /tmp/gcta-1.94.1.zip -d /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/gcta-1.94.1.zip 
#RUN curl -Lo /tmp/locuszoom_1.3.tgz https://statgen.sph.umich.edu/locuszoom/download/locuszoom_1.3.tgz && \
 #   tar zxvf /tmp/locuszoom_1.3.tgz -C /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/locuszoom_1.3.tgz 
#RUN curl -Lo /tmp/finemap_v1.1.tgz http://www.christianbenner.com/finemap_v1.1_x86_64.tgz && \
 #   tar zxvf /tmp/finemap_v1.1.tgz -C /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/finemap_v1.1.tgz 
#RUN curl -Lo /tmp/magma_v1.10.zip https://ctg.cncr.nl/software/MAGMA/prog/magma_v1.10.zip && \
 #   unzip /tmp/magma_v1.10.zip -d /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/magma_v1.10.zip 
#RUN curl -Lo /tmp/annovar.tgz http://www.openbioinformatics.org/annovar/download/0wgxR2rIVP/annovar.latest.tar.gz && \
 #   tar zxvf /tmp/annovar.tgz -C /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/annovar.tgz 
#RUN curl -Lo /tmp/PRSice_2.3.5.zip https://github.com/choishingwan/PRSice/releases/download/2.3.5/PRSice_linux.zip && \
 #   unzip /tmp/PRSice_2.3.5.zip -d /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/PRSice_2.3.5.zip
#RUN curl -Lo /tmp/mach_1.0.18.tgz http://csg.sph.umich.edu/abecasis/MaCH/download/mach.1.0.18.Linux.tgz && \
 #   tar zxvf /tmp/mach_1.0.18.tgz -C /ricopili/dependencies/ && \
  #  chmod 755 /ricopili/dependencies/ && \
   # rm /tmp/mach_1.0.18.tgz
#RUN cd /tmp/ && git clone https://github.com/fhormoz/caviar.git
 #   #cd /tmp/caviar/CAVIAR-C++ && make && make install && \
  #  #cp -r /tmp/caviar /ricopili/dependencies/ && \
   # #chmod 755 /ricopili/dependencies/ && \
    ##rm -r /tmp/caviar   

#MiXeR - (was MATLAB and) Python-based 
#RUN cd /tmp/ && git clone --recurse-submodules -j8 https://github.com/precimed/mixer.git && \
 #   mkdir mixer/src/build && cd mixer/src/build && \
  #  #cmake .. && make bgmg -j16 && \                                # if you use GCC compiler
   # #CC=icc CXX=icpc cmake .. && make bgmg -j16 && \                # if you use Intel compiler
    ##cmake .. -DBOOST_ROOT=$HOME/boost_1_69_0 && make bgmg -j16 && \  # if you use locally compiled boost
    #cp -r /tmp/mixer /ricopili/dependencies/ && rm -r /tmp/mixer
#pleiofdr - MATLAB and Python-based
#RUN cd /tmp/ && git clone https://github.com/precimed/pleiofdr.git && \
 #   cd /tmp/pleiofdr && \
  #  wget https://precimed.s3-eu-west-1.amazonaws.com/pleiofdr/pleioFDR_demo_data.tar.gz && \
   # tar -xzvf pleioFDR_demo_data.tar.gz && \
    ##matlab -nodisplay -nosplash < runme.m
    #cp -r /tmp/pleiofdr /ricopili/dependencies/ && rm -r /tmp/pleiofdr
#mostest - MATLAB and Python-based
#RUN cd /tmp/ && git clone https://github.com/precimed/mostest.git && \
 #   cp -r /tmp/mostest /ricopili/dependencies/ && rm -r /tmp/mostest

#Original dependencies
ENV PATH=/ricopili/dependencies/Minimac3:/ricopili/dependencies/bcftools:/ricopili/dependencies/bgzip:/ricopili/dependencies/eagle:/ricopili/dependencies/eigensoft/EIG-6.1.4/bin:/ricopili/dependencies/impute_v2:/ricopili/dependencies/impute_v4:/ricopili/dependencies/latex:/ricopili/dependencies/ldsc:/ricopili/dependencies/liftover:/ricopili/dependencies/metal:/ricopili/dependencies/shapeit:/ricopili/dependencies/shapeit3:/ricopili/dependencies/tabix:$PATH
#Added by me
ENV PATH=/ricopili/dependencies/bedtools-2.30.0:/ricopili/dependencies/vcftools:/ricopili/dependencies/caviar:/ricopili/dependencies/PAINTOR_V3.0:/ricopili/dependencies/gcta-1.94.1:/ricopili/dependencies/locuszoom_1.3:/ricopili/dependencies/finemap_v1.1:/ricopili/dependencies/magma_v1.10:/ricopili/dependencies/annovar:/ricopili/dependencies/PRSice_2.3.5:/ricopili/dependencies/mach_1.0.18:/ricopili/dependencies/pleiofdr:/ricopili/dependencies/mixer:/ricopili/dependencies/mostest:$PATH

EXPOSE 8888
ENTRYPOINT ["jupyter", "lab", "--allow-root", "--ip=0.0.0.0", "--port=8888", "--no-browser"]
CMD ["/bin/bash"]
