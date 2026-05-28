# Robot Differential Drive Dual Mode

Proyek ini berisi program Arduino untuk robot **differential drive** berbasis board ESP32-compatible. Robot dapat bergerak menuju koordinat target dengan dua strategi navigasi, membaca odometri dari encoder roda, menerima input melalui keypad, menampilkan status pada LCD I2C, serta mengoperasikan gripper servo dan mekanisme lift.

## Fitur Utama

- Dua mode navigasi:
  - **Mode 1 - Kurva:** robot bergerak langsung menuju target menggunakan koreksi arah secara kontinu.
  - **Mode 2 - Rotasi + Maju:** robot berotasi terlebih dahulu ke arah target, lalu bergerak maju.
- Input koordinat target X dan Y melalui keypad 4x3 dalam satuan cm.
- Perhitungan odometri menggunakan encoder roda kiri dan kanan.
- Kontrol kecepatan berbasis proportional control untuk roda.
- Tampilan mode, proses, dan status robot melalui LCD I2C 16x2.
- Gripper servo untuk menjepit dan melepas objek.
- Mekanisme lift untuk menaikkan dan menurunkan objek.
- Return home setelah objek berhasil diambil.

## Hardware yang Digunakan

- Board ESP32-compatible.
- 2 motor DC untuk sistem differential drive.
- Driver motor untuk motor kiri dan kanan.
- Encoder roda kiri dan kanan.
- LCD I2C 16x2.
- Keypad matrix 4x3.
- Servo gripper.
- Motor/aktuator lift dengan driver.
- Catu daya sesuai kebutuhan motor, servo, dan board kontrol.

## Library Arduino

Pastikan library berikut tersedia sebelum meng-upload program:

- `Wire`
- `LiquidCrystal_I2C`
- `Keypad`
- `ESP32Servo`
- `math`

Library `Wire` dan `math` biasanya sudah tersedia bersama core Arduino/ESP32. Library lain dapat dipasang melalui **Arduino IDE > Library Manager**.

## Pinout

### LCD I2C dan Keypad

| Komponen | Pin / Alamat |
| --- | --- |
| LCD I2C address | `0x27` |
| LCD SDA | `21` |
| LCD SCL | `47` |
| Keypad baris 1 | `8` |
| Keypad baris 2 | `18` |
| Keypad baris 3 | `17` |
| Keypad baris 4 | `16` |
| Keypad kolom 1 | `15` |
| Keypad kolom 2 | `7` |
| Keypad kolom 3 | `6` |

### Motor dan Encoder

| Fungsi | Pin |
| --- | --- |
| Motor kiri maju | `10` |
| Motor kiri mundur | `11` |
| Motor kiri PWM | `9` |
| Motor kanan maju | `13` |
| Motor kanan mundur | `14` |
| Motor kanan PWM | `12` |
| Encoder kiri | `35` |
| Encoder kanan | `36` |

### Gripper dan Lift

| Fungsi | Pin |
| --- | --- |
| Servo gripper | `4` |
| Lift naik | `38` |
| Lift turun | `39` |
| Lift PWM | `37` |

> Catatan: pastikan board yang digunakan mendukung nomor GPIO tersebut, PWM, dan interrupt pada pin encoder.

## Parameter Robot

| Parameter | Nilai |
| --- | --- |
| Diameter roda | `0.065 m` |
| Radius roda | `0.0325 m` |
| Jarak antar roda | `0.142 m` |
| Encoder per motor revolution | `25` tick |
| Kecepatan linear maksimum | `0.25 m/s` |
| Kecepatan angular maksimum | `3.5 rad/s` |
| PWM maksimum | `255` |
| `Kp_linear` | `3.5` |
| `Kp_angular` | `3.9` |
| `Kp_wheel` | `1.6` |
| Feed-forward kiri | `0.88` |
| Feed-forward kanan | `1.00` |

## Instalasi dan Upload

1. Buka Arduino IDE.
2. Pastikan board ESP32 sudah terpasang di Board Manager.
3. Install library yang dibutuhkan melalui Library Manager.
4. Buka file `DONEEE.ino`.
5. Pilih board dan port yang sesuai.
6. Upload program ke board.
7. Hubungkan rangkaian sesuai pinout dan pastikan catu daya motor/servo mencukupi.

## Cara Penggunaan

1. Nyalakan robot.
2. Pilih mode melalui keypad:
   - Tekan `1` untuk mode **Kurva**.
   - Tekan `2` untuk mode **Rotasi + Maju**.
3. Masukkan target koordinat `X` dalam cm.
4. Tekan `#` untuk mengonfirmasi input.
5. Masukkan target koordinat `Y` dalam cm.
6. Tekan `#` untuk mengonfirmasi input.
7. Gunakan `*` untuk menghapus input yang sedang diketik.
8. Robot akan menuju target, menjepit objek, mengangkat lift, kembali ke home, menurunkan lift, lalu melepas objek.

## Alur Kerja Program

1. Robot menampilkan pesan awal pada LCD.
2. Pengguna memilih mode navigasi.
3. Pengguna memasukkan koordinat target.
4. Program mereset odometri dan state navigasi.
5. Robot bergerak menuju target sesuai mode yang dipilih.
6. Robot menjalankan sequence gripper dan lift untuk mengambil objek.
7. Robot kembali ke posisi home.
8. Robot menurunkan lift, melepas objek, dan menampilkan status selesai.

## Catatan Kalibrasi

- Nilai `ffGainL` dan `ffGainR` digunakan untuk mengompensasi perbedaan karakteristik motor kiri dan kanan.
- Toleransi jarak dan sudut pada setiap mode dapat disesuaikan jika robot terlalu cepat berhenti atau terlalu lama melakukan koreksi.
- Nilai PWM minimum pada fase rotasi dan maju dapat dituning sesuai torsi awal motor.
- Durasi `liftNaik()`, `liftTurun()`, dan `balikHome_Sederhana()` perlu dikalibrasi berdasarkan mekanik robot sebenarnya.
- Jika posisi akhir tidak akurat, periksa diameter roda, jarak antar roda, resolusi encoder, slip roda, dan arah pemasangan motor.

## Struktur Repository

```text
.
|-- DONEEE.ino
`-- README.md
```

## Lisensi

Tambahkan lisensi sesuai kebutuhan sebelum repository dipublikasikan. Jika tidak ada batasan khusus, proyek ini dapat menggunakan **MIT License**.
