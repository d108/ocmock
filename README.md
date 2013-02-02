dz1111-OCMock
=============

This is a fork off of erikdoe/ocmock for the purpose of building a combined static library for unit testing, using OCMock, on iOS devices and iOS simulators.

More info on OCMock at [ocmock.org](ocmock.org).

Building a combined static library is complicated enough that it warrants more detailed instructions. That is what I aim to provide here.

Using Xcode 4.6 and the iOS 6.1 SDK, I update the build settings to get rid of the warnings.

When I build the OCMockLib target, the libOCMock.a product remains red-colored. However, it has been built successfully when I go to the build directory specified in the Locations section of Xcode's preferences.First build is for `$(ARCHS_STANDARD_32_BIT)`.Set build scheme to OCMockLib > [Physical Device]Second build is for i386. Add i386 architecture to Valid Architectures.Set build scheme to OCMockLib > iPhone 6.1 Simulator.
The result is two `libOCMock.a` static library files.
These can be combined with `lipo`.    $ ls
    Debug-iphoneos/        Debug-iphonesimulator/

    $ lipo -create Debug-iphoneos/libOCMock.a Debug-iphonesimulator/libOCMock.a -output libOCMock.a
    
The combined binary result is [libOCMock.a](https://github.com/dz1111/ocmock/blob/master/Binary/libOCMock.a). This library can be added to your project.

It can be verified using 
    
    $ lipo -info libOCMock.a 
    Architectures in the fat file: libOCMock.a are: armv7 armv7s i386 > Finally and most importantly Xcode needs to tell the compiler to force load the OCMock libraries at compile time, otherwise your tests will crash. Within the Build Settings in your test build target add the following line to the Other Linker Flags entry; -force_load $(SRCROOT)/tests/libraries/libOCMock.a.Ref: http://def.reyssi.net/blog/2012/03/23/using-ocmock-with-xcode4/


