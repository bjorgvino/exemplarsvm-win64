This fork contains a few changes in the MATLAB and C++ code to get it to compile on Windows 7 64 bit.

# The environment
Windows 7 64 bit
Microsoft Visual C++ 2010 (run 'mex -setup' in MATLAB to configure the compiler)
MATLAB 7.12.0 R2011a 64 bit
wget (used in esvm_demo_apply.m, you can get it at http://gnuwin32.sourceforge.net/packages/wget.htm)

--- 

# Change notes (just some notes I wrote down when I was figuring this thing out...)
Setup compiler for MATLAB
- Run 'mex -setup' in MATLAB command window and select a compiler (I used Microsoft Visual C++ 2010)
Install wget for Windows (used in the demos)
- See http://gnuwin32.sourceforge.net/packages/wget.htm
- Don't forget to add the bin folder to the PATH environment variable in Windows (test by opening cmd.exe and running 'wget')
- You need to restart MATLAB after you change the PATH environment variable
Replace "bzero(a,b)" with "memset(a,0,b)" where needed:
- features/resize.cc
Add "static inline double round(double x) { return (floor(x + 0.5)); }" where needed:
- features/resize.cc
- features/features_raw.cc
- features/features_pedro.cc
Replace "mex -largeArrayDims -O libsvmtrain.c svm.o svm_model_matlab.o" with "mex -largeArrayDims -O libsvmtrain.c svm.obj svm_model_matlab.obj" in libsvm/libsvm_compile.m
Remove pthreads from fconvblas.cc since pthreads-win32 for Windows is not 64 bit and therefore doesn't work.
- remove "#include<pthread.h>"
- replace "pthread_exit(NULL);" with "return NULL;"
- remove "pthread_t *ts = (pthread_t *)mxCalloc(len, sizeof(pthread_t));"
- replace "if (pthread_create(&ts[i], NULL, process, (void *)&td[i])) mexErrMsgTxt("fconvblas.cc: Error creating thread");" with "process((void *)&td[i]);"
- remove "if (skips[i] == false) pthread_join(ts[i], &status);"
- remove "mxFree(ts);"
See: http://www.mathworks.se/support/solutions/en/data/1-36Z527/index.html?product=ML&solution=1-36Z527

---

**Updated by Abhinav Shrivastava**

**Copyright (C) 2011 by Tomasz Malisiewicz**

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
