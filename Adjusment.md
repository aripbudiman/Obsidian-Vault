| Field                   | Type                  | Keterangan                               |
| ----------------------- | --------------------- | ---------------------------------------- |
| employee_number         | varchar(50)           | NIP otomatis format `{tahun}{increment}` |
| employment_status_id    | bigint FK             | Relasi ke Master Status Kepegawaian      |
| join_date               | date                  | Tanggal bergabung                        |
| resign_date             | date nullable         | Tanggal resign                           |
| employee_status         | varchar(20)           | active / inactive                        |
| payroll_enabled         | boolean               | Flag payroll aktif                       |
| bank_name               | varchar(100) nullable | Nama bank                                |
| bank_account_number     | varchar(100) nullable | Nomor rekening                           |
| bank_account_name       | varchar(150) nullable | Nama pemilik rekening                    |
| npwp                    | varchar(100) nullable | NPWP                                     |
| pph21_enabled           | boolean               | PPh21 aktif                              |
| tax_ptkp_status_id      | bigint nullable       | Relasi ke Status PTKP                    |
| tax_method              | varchar(50) nullable  | gross / gross_up / nett / manual         |
| bpjs_health_enabled     | boolean               | BPJS kesehatan aktif                     |
| bpjs_health_number      | varchar(100) nullable | Nomor BPJS kesehatan                     |
| bpjs_employment_enabled | boolean               | BPJS ketenagakerjaan aktif               |
| bpjs_employment_number  | varchar(100) nullable | Nomor BPJS ketenagakerjaan               |
| payroll_notes           | text nullable         | Catatan payroll                          |