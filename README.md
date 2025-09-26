# ECSS-SMP-C
C Wrapper for the ESA ECSS SMP Standard.

ECSS version: [ECSS SMP L1 (31 March 2025)](https://ecss.nl/wp-content/uploads/2025/08/ECSS_SMP_L1_(31March2025).zip).

Generated using [CWraPPer](https://github.com/abadiet/CWraPPer/).

> [!WARNING]
> The SMP standard is not fully wrapped: templated objects, constructors and some operators.
> 
> Some definitions have been slighty change (e.g. use of pointer for class passed as argument).

## Generate
```bash
# Clone CWraPPer
git clone https://github.com/abadiet/CWraPPer/

# Create the dummy main.cpp for the lib
./cWrapper/buildMainCpp.sh ./Smp # assuming the CPP headers are in ./Smp

# Build CWraPPer
mkdir build
cmake -S CWraPPer -B build -DCWRAPPER_DEBUG=ON

# Run CWraPPer: for MacOS, may be slightly different for your configuration
./build/cwrapper Smp Smp-C Smp/main.cpp -- \
    -x c++ \
    -std=c++17 \
    -resource-dir $(xcrun --show-toolchain-path)/usr/lib/clang/17 \
    -isysroot $(xcrun --show-sdk-path) \
    -I $(pwd) \
    -Xclang -internal-isystem $(xcrun --show-sdk-path)/usr/include/c++/v1 \
    -Xclang -internal-isystem $(xcrun --show-sdk-path)/usr/local/include \
    -Xclang -internal-isystem $(xcrun --show-toolchain-path)/usr/lib/clang/17/include \
    -Xclang -internal-externc-isystem $(xcrun --show-sdk-path)/usr/include \
    -Xclang -internal-externc-isystem $(xcrun --show-toolchain-path)/usr/include \
    -Xclang -internal-iframework $(xcrun --show-sdk-path)/System/Library/Frameworks \
    -Xclang -internal-iframework $(xcrun --show-sdk-path)/System/Library/SubFrameworks \
    -Xclang -internal-iframework $(xcrun --show-sdk-path)/Library/Frameworks

# Clean
mkdir include
mv Smp-C include/
cp -r include/Smp-C src
rm include/Smp-C/**/*.cpp src/**/*.h
mv src/Services/*.cpp src/Publication/*.cpp src/
rm -r src/Publication src/Services
rm -r build Smp/main.cpp

# Fix issues
# 1- Converts CPP includes to their C equivalents
# 2- Take a coffee
# 3- Fix again
```
