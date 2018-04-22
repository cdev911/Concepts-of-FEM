# FEM
## To Do List
- [x] Make a to do list

### Other Things to consider:
- Non overlapping memory transfer for Ryzen/Epyc
- **Don't have to consider kernel benchmarks anymore!!!**

## Progress
- successfull login to hosts `skylakesp2` and `naples` and first run of `likwid-bench`
- In core performance model for vector triad
- Measuremtn of vector triad on Emmy (checked the performance model)
- OpenMP parallelization of vector triad on Emmy
- Scripts for running and plotting created
- Created Latex slides template

# Ivy Bridge

### Peak Performance

|               |               |
| ---           | ---           |
| AVX	        | 1LD & 0.5ST	|
| Add	        | 1/cy			|
| Mul	        | 1/cy			|
| FMA	        | n/a			|
| Peak mem BW   | 51.2 GB/s     |
| Load-only BW   | 46.1 GB/s     |


## Random
- for skylake/epyc execution:
    - `ssh testfront` - `pbsnodes -a` - `qsub -I -l nodes=skylakesp2:noturbo,walltime=00:10:00`
- skylake set frequency:
    - `module load likwid`
    - `likwid-setFrequencies -l` for a list of possible frequencies
    - `likwid-setFrequencies -p` for a list of all core and uncore frequencies
    - `likwid-setFrequencies -f 2.4` set all cores to 2.4 GHz
    - `likwid-setFrequencies --umin 2.4` set uncore min frequency to 2.4 (same as max uncore frequ)
    - `likwid-pin -s 0x0 -c E:N:1:1:2 ./triad 900` pin thread to first core
- to use likwid perfctr:
    - `module load likwid`
    - `likwid-perfctr -C S0:1 -s 0x0 -g L3 -O -m ./read <elements> <allig> <num reads>` to do measurements in L3 for example, thread pinned to second core on Socket 0 and skip shepard thread (0x0)
    - `qsub -l nodes=X:ppn=40:likwid -l walltime=XX:XX:XX <batch-script>` you have to set the property likwid when requesting a job
