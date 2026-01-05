# PetaBlitz
A tool for GPU compression-base archives



(actually working on the first ever release)

The software is engineered like this:

┌─────────────────────────────────────────────────────────────────────┐
│  Host (CPU) – Windows 11                                            │
│  ├─ File I/O (std::ifstream / ofstream)                           │
│  ├─ Input Validation (size, CRC32)                                 │
│  ├─ Buffer Partition → Fixed‑Size Blocks (e.g. 64 KB)             │
│  ├─ GPU Resource Creation (DX12 Compute Buffer, UAV)            │
│  ├─ Root‑Signature & Descriptor‑Heap Setup                        │
│  ├─ Dispatch Compute Shader (Compression)                        │
│  ├─ Dispatch Compute Shader (Decompression)                     │
│  ├─ Retrieve Metadata → Write Output File                       │
│  └─ Logging / Error‑Handling (spdlog + HIP‑CHECK‑style)          │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│  GPU (DX12 Compute Shader)                                         │
│  ├─ __global__ compressKernel (block‑wise LZ4‑style)             │
│  ├─ __global__ decompressKernel (inverse)                        │
│  ├─ Shared‑Memory (optional sliding‑window)                     │
│  ├─ Coalesced Reads/Writes → Max Bandwidth                     │
│  └─ Metadata Array (origLen, compLen, blockIdx)                 │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│  UI Layer (WinUI 3 / WPF)                                          │
│  ├─ FilePicker (System‑File‑Dialog)                               │
│  ├─ Progress‑Bar (Animated GPU‑Utilisation)                     │
│  ├─ Real‑Time Visualisation (heat‑map of blocks)               │
│  └─ Export / Save‑As (compressed .bin)                           │
└─────────────────────────────────────────────────────────────────────┘
