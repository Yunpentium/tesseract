# Installing Tesseract on WSL(ubuntu18.04  gcc7.5  cmake3.10.2)

## 1 fix dependency

### 1.1 leptonica
    git clone -b master https://github.com/danbloomberg/leptonica/releases
    mkdir build
    cd build
    cmake .. -DBUILD_SHARED_LIBS=ON   //Fix permission issue, https://github.com/DanBloomberg/leptonica/issues/536
    make -j
    sudo make install

### 1.2 ICU 
    sudo apt-get install libicu-dev -y
  
### 1.3 pango  >=1.22.0 
    Pango is a library for laying out and rendering of text, with an emphasis on internationalization
    wget http://ftp.gnome.org/pub/gnome/sources/pango/1.28/pango-1.28.1.tar.gz  // If download fail, get from website manually. Newest version build by meson.
    tar -xzvf pango-1.28.1.tar.gz
    cd pango-1.28.1/
    ./configure --prefix=/usr
    make -j
    sudo make install
    
 ### 1.4 cairo   
    sudo apt-get install libcairo2-dev -y
    
  ### 1.5 pangoft2  
    sudo apt-get install libpango1.0-dev
    
    
## 2 build and install
    cmake ..
    make -j
    sudo make install   //    /usr/local/lib/libtesseract.so -> libtesseract.so.5.0.0
                        //    /usr/local/lib/libtesseract.so.5.0.0
    sudo ldconfig
    
 ## 3 Running tesseract
    tesseract --help
    
    
