*wxHaskell* is a good library to do GUI programming in Haskell. However, *wx* has not been updated since last upload. For the latest **GHC**, *wx* cannot be compiled successfully. The main issue is related to the API changes of *Cabal* package, which is used in the **setup.hs** of the packages *wxc* and *wxcore*. Therefore, if you use the latest **GHC**, such as **GHC 8.10.7** or **GHC 9.0.1**, you cannot install *wx* from Hackage.

This repo includes the *wxc* and *wxcore* packages, which are modified according to the latest APIs of the *Cabal* packages so that *wx* can be installed with the latest GHC. The complete installing process in Windows OS is as follows:

1. Install *wxWidgets*. I recommend that installing *wxWidgets* from **msys2**:

   ```shell
   pacman -S mingw-w64-x86_64-wxWidgets
   ```

   Please make sure that the installed `mingw-w64-x86_64-wxWidgets`  is of version 3.0 because `wx` can only work with `wxWidgets 2.9` and `3.0`. After installing, you can run `wx-config --version` to check it.

   ```shell
   wx-config --release
   3.0
   ```

   Another important thing is that the version of the compiler used to compile `wxWidgets` should be compatible with the `gcc` or `g++` version bundled with `GHC`, otherwise, there will be a compatibility issue between the app you build with `GHC` and the `wxWidgets` library.

   For example,  `mingw-w64-x86_64-wxWidgets-3.0.4` (build date is 2019-5-31) is compatible with `GHC 8.10` and `mingw-w64-x86_64-wxWidgets-3.0.5` (build date is 2021-10-29) is compatible with `GHC 9`.

2. Create a new `wx` project such as:

   ```shell
   stack new wx-learning
   ```

3. Editor the `stack.yaml` file by adding the corresponding packages.

   ```haskell
   extra-deps:
   - wx-0.92.3.0@sha256:c4685d4f1fcb92e279409eb9455bef41cbd336d0b557040a6966bcf4881124a1,1825
   - wxdirect-0.92.3.0@sha256:7533e84784560be0313b5940227870ea105975d581eab5b6ad5e8219d82dfaff,1740
   - git: git@github.com:chenyulue/wx-packages.git
     commit: 2291dbb134c2604707aa21698ab75f8820a52fca
     subdirs:
       - wxc
       - wxcore
   
   allow-newer: true
   ```

3.  Run `stack build`