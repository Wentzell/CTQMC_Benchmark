# Instructions for the Setup of a CTHyb Performance Benchmark

## Software Environment

Clang 16 + MKL

```bash
export MODULEPATH=/mnt/home/wentzell/opt/modules:$MODULEPATH
module load devenv6/clang-py3-mkl
```

## Software Setup (TRIQS + cthyb + tprf)

```bash
INSTALL_PREFIX=$PWD/install
NCORES=40

for i in triqs cthyb tprf; do
  git clone https://github.com/TRIQS/$i $i.src --branch=3.2.x --depth 1
  mkdir -p $i.build && cd $i.build
  cmake ../$i.src -DCMAKE_INSTALL_PREFIX=$INSTALL_PREFIX
  make -j$NCORES && ctest -j$NCORES && make install
  cd ../
  source $INSTALL_PREFIX/share/triqsvars.sh
done
```

## Fetching the benchmark

```bash
git clone https://github.com/TRIQS/benchmarks --depth 1
cd benchmarks/Sr2RuO4/scripts
```

## Running the benchmark

```bash
mpirun cthyb 2> /dev/null | grep "Simulation lasted"
```
