# Instructions for the Setup of a CTHyb Performance Benchmark

## Software Environment

Clang 9.0.0 + OpenBlas

```bash
module load gcc/7.4.0 cmake/3.14.5 llvm/9.0.0 openmpi2/2.1.6-hfi python2/2.7.16 python2-mpi4py/2.7.16-openmpi2
module load lib/openblas/0.3.6 lib/fftw3/3.3.8-openmpi2 lib/hdf5/1.8.21-openmpi2 lib/NFFT/3.4.0 lib/boost/1.70-gcc7-openmpi2
```

## Software Setup (TRIQS + cthyb + tprf)

```bash
export CC=clang
export CXX=clang++
export CXXFLAGS="-march=broadwell -O3 -stdlib=libc++"
INSTALL_PREFIX=$PWD/install
NCORES=40

for i in triqs cthyb tprf; do
  git clone https://github.com/TRIQS/$i $i.src --branch=2.2.x --depth 1
  mkdir -p $i.build && cd $i.build
  cmake ../$i.src -DCMAKE_INSTALL_PREFIX=$INSTALL_PREFIX
  make -j$NCORES && ctest -j$NCORES && make install
  cd ../
  source $INSTALL_PREFIX/share/triqsvars.sh
done
```

## Running the benchmark
```bash
git clone https://github.com/TRIQS/benchmarks --depth 1
cd benchmarks/Sr2RuO4/scripts
mpirun -np 128 cthyb | grep "Simulation lasted"
```
