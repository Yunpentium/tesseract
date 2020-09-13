# Tesseract on WSL(ubuntu18.04  gcc7.5  cmake3.10.2)

## 1 fix dependency

### 1.1 leptonica
    Leptonica它依赖于开源的zlib、libjpeg、libpng、libtiff、giflib的.so, 如果缺少一定要安装上，apt-get或源码
        echo y | sudo apt-get install zlib1g-dev
        echo y | sudo apt-get install libpng-dev
        echo y | sudo apt-get install libgif-dev
        echo y | sudo apt-get install libjpeg-dev
        echo y | sudo apt-get install libtiff-dev
        
    git clone -b master https://github.com/danbloomberg/leptonica/releases
    mkdir build
    cd build
    cmake .. -DBUILD_SHARED_LIBS=ON   //Fix permission issue, see https://github.com/DanBloomberg/leptonica/issues/536
        root@A191130469:/mnt/d/project/other/leptonica/build# cmake .. -DBUILD_SHARED_LIBS=ON
        -- Found PNG: /usr/local/lib/libpng.so (found version "1.6.37")
        -- Can not find: /webp/mux.h
        CMAKE_BUILD_TYPE = Release
        --
        -- General configuration for Leptonica 1.81.0
        -- --------------------------------------------------------
        -- Build type: Release
        -- Compiler: GNU
        -- C compiler options:
        -- Linker options:
        -- Install directory: /usr/local
        --
        -- Build with sw [SW_BUILD]: OFF
        -- Build utility programs [BUILD_PROG]: OFF
        -- Used ZLIB library: /usr/lib/x86_64-linux-gnu/libz.so
        -- Used PNG library:  /usr/local/lib/libpng.so;/usr/lib/x86_64-linux-gnu/libz.so
        -- Used JPEG library: /usr/local/lib/libjpeg.so
        -- Used JP2K library:
        -- Used TIFF library: /usr/local/lib/libtiff.so
        -- Used GIF library:  /usr/lib/x86_64-linux-gnu/libgif.so
        -- Used WEBP library:
        -- --------------------------------------------------------
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
 
### 3.1  command line  
    Default image format for firstly tesseract version was .tif or .tiff. in new version you should install following format package (libgif libjpeg libpng libtiff zlib). Leptonica use this pakages for read images and tesseract use leptonica for analyse images.
    
    root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# tesseract -v
        tesseract 5.0.0-alpha
         leptonica-1.81.0
          libgif 5.1.4 : libjpeg 9d : libpng 1.6.37 : libtiff 4.1.0 : zlib 1.2.11
         Found AVX2
         Found AVX
         Found FMA
         Found SSE
     root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# tesseract --list-langs
        List of available languages (2):
        eng
        osd
     root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# tesseract --help
        Usage:
          tesseract --help | --help-extra | --version
          tesseract --list-langs
          tesseract imagename outputbase [options...] [configfile...]

        OCR options:
          -l LANG[+LANG]        Specify language(s) used for OCR.
        NOTE: These options must occur before any configfile.

        Single options:
          --help                Show this help message.
          --help-extra          Show extra help for advanced users.
          --version             Show version information.
          --list-langs          List available languages for tesseract engine.


### 3.2  work well for PNG in English
        snapshot in.PNG from https://en.wikipedia.org/wiki/Main_Page
        
        root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# tesseract in.PNG out
        Tesseract Open Source OCR Engine v5.0.0-alpha with Leptonica
        root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# cat out
        cat: out: No such file or directory
        root@A191130469:/mnt/d/project/code/cpp_clion/demo/code/src/oj/res# cat out.txt
        Today's featured picture



        Nogi Maresuke (25 December 1849 - 13 September 1912) was a general in the Imperial
        Japanese Army and a governor-general of Taiwan. His career included action in the Satsuma
        Rebellion and the First Sino-Japanese War. He was recalled to active service in the Russo-
        Japanese War, and made a report directly to Emperor Meiji during a Gozen Kaig/ at the end of the
        war. When explaining battles during the Siege of Port Arthur in detail, he broke down and wept,
        apologizing for the 56,000 lives lost in the campaign and asking to be allowed to kill himself in
        atonement. The emperor told him that suicide was unacceptable, as all responsibility for the war
        was due to imperial orders, and that Nogi must remain alive, at least as long as he himself lived.
        Emperor Meiji died in 1912, and Nogi and his wife Shizuko committed suicide shortly after the
        funeral cortége left the palace.

        Photograph credit: unknown; restored by Adam Cuerden

        Recently featured: Spotted hyena + Masih Alinejad + Nicholas Lanier
        Archive ¢ More featured pictures

