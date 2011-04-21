python-zxing - a quick and dirty wrapper fo the [ZXing barcode library](http://code.google.com/p/zxing/)

This is a hack subprocess control library that gives you a reasonable Python interface to the ZXing command line.  ZXing will recognize and decode 1D and 2D
barcodes in images, and return position information and decoded values.  This
will let you read barcodes from any images in Python.

If you need to threshold or filter your image prior to sending to ZXing, I 
recommend using functions from (SimpleCV)[http://sf.net/p/simplecv].

Instructions for use

    wget http://code.google.com/p/zxing/downloads/detail?name=ZXing-1.6.zip
    cd zxing-1.6 
    ant -f core/build.xml
    ant -f javase2/build.xml 
    git clone git://github.com/oostendo/python-zxing.git
    cd python-zxing
    nosetest tests.py

The library consists of two classes, BarCodeReader and BarCode.  BarCode parses
the output from ZXing's CommandLineRunner into a BarCode object which includes:

    b = zxing.BarCode("""
    file:default.png (format: FAKE_DATA, type: TEXT):
    Raw result:
    foo-bar|the bar of foo
    Parsed result:
    foo-bar 
    the bar of foo
    Also, there were 4 result points:
      Point 0: (24.0,18.0)
      Point 1: (21.0,196.0)
      Point 2: (201.0,198.0)
      Point 3: (205.23952,21.0)
    """)
    
    print b.datatype #FAKE_DATA
    print b.raw #foo-bar|the bar of foo
    print b.data #foo-bar\nthe bar of foo
    print b.points #[(24.0, 18.0) ... ]

Initializing the barcode reader, you have to tell it where to find the ZXing
core modules.  It is the single parameter, which defaults to the parent 
directory.

    reader = zxing.BarCodeReader("/var/opt/zxing")

    barcode = reader.decode("/tmp/image.jpg")
    (barcode1, barcode2) = reader.decode(["/tmp/1.png", "/tmp/2.png"])
    code_list = reader.decode("/tmp/barcodes", True)

decode() takes an image path, directory, or list of images and has an optional parameter to use the "try_harder" option.  If no barcode is found, it returns None objects. 

Installation is manual. 
