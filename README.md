# pWASM-abigen

ABI code and data generator for pWASM VM

# platon-abigen
The tool mainly generates ABI entry C functions for C++ contract files, which have been described by ABI JSON.

## Compile

### Project dependence
* LLVM4.0
* Clang4.0
* Boost 1.65+
* RapidJson

### Dependent compilation
* LLVM4.0 + Clang4.0

```
1.Put the Clang source package under llvm/tools.
2. Create a build directory
3. cmake  -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="all" -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_TESTS=OFF -DLLVM_ENABLE_RTTI=ON -DCMAKE_INSTALL_PREFIX=""
```

 **Windows Mingw**

```
cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86" -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_TESTS=OFF -DLLVM_ENABLE_RTTI=ON  -DCMAKE_INSTALL_PREFIX="C:\chain\llvm4.0" -G "MinGW Makefiles" -DCMAKE_SH="CMAKE_SH-NOTFOUND"
```


* BOOST

```
./bootstrap.sh
./b2 --with-filesystem --with-system --with-log --with-exception  --with-thread --with-random
Windows Need to add --layout=system variant=debug address-model=64 to remove the library prefix
```

## Compile platon-abigen 

```
git submodule init
git submodule update
mkdir build && cd build
cmake  -DLLVM_ROOT="" -DCLANG_ROOT="" -DBOOST_ROOT=""
```

## Command parameter

```
-abi-name Generate ABI JSON filename
-extra-arg Compile c++ parameter options such as header file path
-log-level Log level
-log-path Log path
-outpath Output file path
```


## Use example

```
./platon-abigen -extra-arg="-std=c++14"  -extra-arg="-I/home/juzhen/standlib/platonlib" -extra-arg="-Wnon-virtual-dtor -Wconversion" hello.cpp -log-path=./ -log-level=trace
```

* ABI JSON

```
[
    {
        "name": "transfer",
        "inputs": [
            {
                "name": "from",
                "type": "string"
            },
            {
                "name": "to",
                "type": "string"
            },
            {
                "name": "asset",
                "type": "int32"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "string"
            }
        ],
        "constant": "false",
        "type": "function"
    },
    {
        "name": "token",
        "inputs": [
            {
                "type": "int32"
            },
            {
                "type": "string"
            }
        ],
        "type": "event"
    }
]
```

* Generate C ABI code

```
//platon autogen begin
extern "C" { 
    void transfer(char * from,char * to,int asset) {
        platon::Token Token_platon;
        Token_platon.transfer(from,to,asset);
    }
}
//platon autogen end
```

