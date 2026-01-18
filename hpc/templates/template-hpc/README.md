# HPC / CUDA Project Template (WSL)

This repository is a **reusable HPC/CUDA development template** designed for:
- C / C++ systems programming
- MPI (distributed memory)
- OpenMP (shared memory)
- CUDA (GPU kernels)
- Hybrid designs (MPI + OpenMP + CUDA)

It is intended to run inside **WSL (Ubuntu)** using **system toolchains** (not Conda),
and is fully isolated from ML environments.

---

## 📁 Directory Structure

```text
.
├── CMakeLists.txt
├── README.md
├── src/
│   └── main.cpp
├── include/
├── cuda/
│   └── kernels.cu
├── scripts/
│   ├── build.sh
│   └── run.sh
├── build/            # out-of-source build (generated)
└── .gitignore
```

---

## 🧰 Prerequisites (One-time setup)

Make sure the following are installed in WSL:

```bash
sudo apt update
sudo apt install -y \
  build-essential \
  cmake \
  git \
  gdb \
  valgrind \
  htop \
  pkg-config \
  openmpi-bin \
  libopenmpi-dev \
  nvidia-cuda-toolkit
```

Verify:

```bash
gcc --version
mpicc --version
mpirun --version
nvcc --version
```

---

## 🚀 Build Instructions

From the **project root**:

```bash
chmod +x scripts/build.sh scripts/run.sh
./scripts/build.sh
```

This will:
- Create the `build/` directory (if missing)
- Configure the project with CMake
- Compile the executable

---

## ▶️ Run Instructions (MPI)

Default run (4 MPI processes):

```bash
./scripts/run.sh
```

Manual run (custom process count):

```bash
mpirun -np 8 ./build/hpc_app
```

Expected output:

```text
Hello from rank 0 of 4
Hello from rank 1 of 4
Hello from rank 2 of 4
Hello from rank 3 of 4
```

---

## 🧪 Testing & Validation

### 1. MPI correctness test
Change the number of processes:

```bash
mpirun -np 2 ./build/hpc_app
mpirun -np 6 ./build/hpc_app
```

Each rank should print exactly once.

---

### 2. OpenMP test (if enabled in code)

```bash
OMP_NUM_THREADS=4 mpirun -np 2 ./build/hpc_app
```

This validates hybrid MPI + OpenMP execution.

---

### 3. CUDA availability test

```bash
nvcc --version
nvidia-smi
```

If CUDA kernels are added, GPU activity should be visible via:

```bash
watch -n 0.5 nvidia-smi
```

---

## 🛠️ Development Workflow

### Start a new project from template

```bash
cd ~/hpc-dev/projects
cp -r ../templates/template-hpc my_new_project
cd my_new_project
./scripts/build.sh
./scripts/run.sh
```

### Clean rebuild

```bash
rm -rf build
./scripts/build.sh
```

---

## 🧠 Design Principles

- No Conda or Python dependencies
- Uses system MPI and CUDA toolchains
- CMake-based, portable to real HPC clusters
- Matches industry-standard HPC layouts

---

## 🔮 Future Extensions

This template is designed to grow:
- Add CUDA kernels in `cuda/`
- Add OpenMP regions in `src/`
- Add benchmarking/profiling scripts
- Add Slurm job scripts for clusters
- Add CI/CD build checks

---

## 📌 Notes

- Always work inside the WSL filesystem (`/home/...`), not `/mnt/c`
- This setup mirrors real HPC environments used in research and industry

---
