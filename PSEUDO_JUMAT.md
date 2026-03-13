// SESI JUMAT - 13/03/2026
// CASE 1 - Peluruhan Terkopel 

TETAPKAN λ1=1, λ2=1, N1(0)=1, N2(0)=0, T=5
TETAPKAN dt_list = [0.5, 1.0, 2.0, 2.5, 3.0]

// Solusi Analitik (kasus khusus λ1 = λ2)
UNTUK setiap t dalam [0, T]:
    N1_ana(t) = exp(-λ1 · t)
    N2_ana(t) = λ1 · t · exp(-λ1 · t)

// Loop utama per nilai Δt
UNTUK setiap Δt dalam dt_list:

    N = T / Δt                  // jumlah langkah
    t[0] = 0
    N1[0] = N1(0),  N2[0] = N2(0)

    // Iterasi Euler Forward
    UNTUK i = 0, 1, ..., N-1:
        N1[i+1] = N1[i] + Δt · (-λ1 · N1[i])
        N2[i+1] = N2[i] + Δt · ( λ1 · N1[i] - λ2 · N2[i])
        t[i+1]  = t[i] + Δt

    // Tampilkan subplot untuk Δt ini
    PLOT t vs N1_ana  → garis biru putus-putus
    PLOT t vs N2_ana  → garis merah putus-putus
    PLOT t vs N1      → titik biru (numerik)
    PLOT t vs N2      → titik merah (numerik)
    BERI JUDUL subplot: "Δt = {Δt} | {status stabilitas}"

TAMPILKAN semua subplot

// ========================================================================================

// CASE 2 - Osilasi Harmonik
TETAPKAN ω=3.0, x(0)=1.0, v(0)=0.0, T=10.0
TETAPKAN dt_list = [0.01, 0.05, 0.2, 0.4, 0.8]

// Solusi Analitik 
UNTUK setiap t dalam [0, T]:
    x_ana(t) = x0·cos(ω·t) + (v0/ω)·sin(ω·t)

// Solusi Referensi (dt_ref = 0.001) 
x = x0,  v = v0
UNTUK i = 1, ..., T/dt_ref:
    x_baru = x + v · dt_ref
    v_baru = v - ω² · x · dt_ref
    x = x_baru,  v = v_baru
x_ref_akhir = x

// Loop Utama per Nilai Δt
UNTUK setiap Δt dalam dt_list:

    N = T / Δt
    xs[0] = x0,  v = v0

    // Iterasi Euler Forward
    UNTUK i = 0, 1, ..., N-1:
        xs[i+1] = xs[i] + v · Δt
        v       = v - ω² · xs[i] · Δt

    // Hitung error di t = T
    error = |xs[N] - x_ref_akhir|

    // Tampilkan subplot untuk Δt ini
    PLOT t vs x_ana   → garis hitam putus-putus (analitik)
    PLOT t vs xs      → titik berwarna (numerik)
    BERI JUDUL: "dt = {Δt}, ω = 3.0"

// Panel Konvergensi (opsional) 
PLOT log(dt_list) vs log(errors)  → kurva error
PLOT garis slope-1 sebagai referensi orde Euler

TAMPILKAN semua subplot
CETAK tabel: dt | error
