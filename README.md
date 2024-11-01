# Menambahkan data dengan CSV 🗂️
CSV adalah format file yang menyimpan data dalam bentuk tabel sederhana, dimana satu baris data dihitung sebagai satu record data, yang mana antar kolom dari satu baris tadi dipisahkan dengan tanda ";"
## Langkah-langkah mengupload data dengan CSV 📥
### 1. 📊 Membuat data dengan excel, dan disimpan dalam format CSV

![Screenshot (559)](https://github.com/user-attachments/assets/e6274dbe-75c7-4e09-aa20-9e610b66dd2c)

![PBO2](https://github.com/user-attachments/assets/1e770db5-a20d-4208-8f03-5ad1ed3e74f4)

### 2. 🕹️ Menambahkan button UPLOAD dalam desain GUI

![PBO3](https://github.com/user-attachments/assets/6427014e-dfdc-4064-bc37-145a8a99a8b8)

### 3. 🧮 Source code untuk button UPLOAD

    private void btnUpActionPerformed(java.awt.event.ActionEvent evt) {                                      
        JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);
        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("File yang dipilih : " + filePilihan.getAbsolutePath());
            try {
                BufferedReader br = new BufferedReader(new FileReader(filePilihan));
                String baris;
                String pemisah = ";";
                while ((baris = br.readLine()) != null) {
                    String[] matakuliah = baris.split(pemisah);

                    if (matakuliah.length >= 4) {
                        String KodeMK = matakuliah[0];
                        String NamaMK = matakuliah[1];
                        String SKS = matakuliah[2];
                        String SemesterAjar = matakuliah[3];

                        if (!KodeMK.isEmpty() && !NamaMK.isEmpty() && !SKS.isEmpty() && !SemesterAjar.isEmpty()) {
                            try {
                                Class.forName(driver);
                                conn = DriverManager.getConnection(koneksi, user, password);
                                conn.setAutoCommit(false);
                                String sql = "INSERT INTO MataKuliah (KodeMK, NamaMK, SKS, SemesterAjar) VALUES (?, ?, ?, ?)";
                                pstmt = conn.prepareStatement(sql);

                                pstmt.setString(1, KodeMK);
                                pstmt.setString(2, NamaMK);
                                pstmt.setInt(3, Integer.parseInt(SKS));
                                pstmt.setInt(4, Integer.parseInt(SemesterAjar));

                                int k = pstmt.executeUpdate();

                                if (k > 0) {
                                    conn.commit();
                                    JOptionPane.showMessageDialog(null, "Input Data Berhasil!");
                                }
                            } catch (SQLException e) {
                                JOptionPane.showMessageDialog(null, "Input Data Gagal: " + e.getMessage());
                            } catch (ClassNotFoundException ex) {
                                JOptionPane.showMessageDialog(null, "Driver tidak ditemukan: " + ex.getMessage());
                            } finally {
                                try {
                                    if (pstmt != null) {
                                        pstmt.close();
                                    }
                                    if (conn != null) {
                                        conn.close();
                                    }
                                } catch (SQLException closeEx) {
                                    JOptionPane.showMessageDialog(null, "Close Gagal: " + closeEx.getMessage());
                                }
                            }
                        }
                    } else {
                        System.out.println("Baris tidak memiliki cukup kolom: " + baris);
                    }
                }
                br.close();
            } catch (FileNotFoundException ex) {
                Logger.getLogger(MataKuliah.class.getName()).log(Level.SEVERE, null, ex);
            } catch (IOException ex) {
                Logger.getLogger(MataKuliah.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        tampil();
    }

## Implementasi 🕵️‍♂️

### Klik button UPLOAD, maka akan keluar tampilan seperti di bawah:

![PBO4](https://github.com/user-attachments/assets/476134f4-0dd1-4be4-a181-117dbdca1698)

### Pilih file yang akan diupload

![PBO5](https://github.com/user-attachments/assets/612f442c-2766-4b67-87f4-30c2d29ffed7)

### Karena input berhasil, maka akan keluar pesan seperti pada gambar:

![pbo6](https://github.com/user-attachments/assets/2d6d6981-9151-467a-8196-a9f614ed0156)

### Maka data pun telah berhasil dimasukkan

![pbo7](https://github.com/user-attachments/assets/82503f78-4597-4bd0-a46f-1240dee95a52)
