-- form khach hang
Imports System.Data.SqlClient
Public Class frmKhachHang
    Dim conn As SqlConnection = New SqlConnection("Data Source=.;Initial Catalog=QLBanHang;Integrated Security=True")
    Dim dt As DataTable
    Dim da As SqlDataAdapter

    Private Sub frmKhachHang_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        LoadData()
    End Sub

    Private Sub LoadData()
        da = New SqlDataAdapter("SELECT * FROM KhachHang", conn)
        dt = New DataTable()
        da.Fill(dt)
        dgvKhachHang.DataSource = dt
    End Sub

    Private Sub btnThem_Click(sender As Object, e As EventArgs) Handles btnThem.Click
        Dim cmd As New SqlCommand("INSERT INTO KhachHang (TenKH, SDT, DiaChi, LoaiKH) VALUES (@TenKH,@SDT,@DiaChi,@LoaiKH)", conn)
        cmd.Parameters.AddWithValue("@TenKH", txtTenKH.Text)
        cmd.Parameters.AddWithValue("@SDT", txtSDT.Text)
        cmd.Parameters.AddWithValue("@DiaChi", txtDiaChi.Text)
        cmd.Parameters.AddWithValue("@LoaiKH", txtLoaiKH.Text)

        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub

    Private Sub btnSua_Click(sender As Object, e As EventArgs) Handles btnSua.Click
        Dim cmd As New SqlCommand("UPDATE KhachHang SET TenKH=@TenKH, SDT=@SDT, DiaChi=@DiaChi, LoaiKH=@LoaiKH WHERE MaKH=@MaKH", conn)
        cmd.Parameters.AddWithValue("@MaKH", Integer.Parse(txtMaKH.Text))
        cmd.Parameters.AddWithValue("@TenKH", txtTenKH.Text)
        cmd.Parameters.AddWithValue("@SDT", txtSDT.Text)
        cmd.Parameters.AddWithValue("@DiaChi", txtDiaChi.Text)
        cmd.Parameters.AddWithValue("@LoaiKH", txtLoaiKH.Text)

        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub

    Private Sub btnXoa_Click(sender As Object, e As EventArgs) Handles btnXoa.Click
        Dim cmd As New SqlCommand("DELETE FROM KhachHang WHERE MaKH=@MaKH", conn)
        cmd.Parameters.AddWithValue("@MaKH", Integer.Parse(txtMaKH.Text))

        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub

    Private Sub dgvKhachHang_CellClick(sender As Object, e As DataGridViewCellEventArgs) Handles dgvKhachHang.CellClick
        If e.RowIndex >= 0 Then
            Dim row As DataGridViewRow = dgvKhachHang.Rows(e.RowIndex)
            txtMaKH.Text = row.Cells("MaKH").Value.ToString()
            txtTenKH.Text = row.Cells("TenKH").Value.ToString()
            txtSDT.Text = row.Cells("SDT").Value.ToString()
            txtDiaChi.Text = row.Cells("DiaChi").Value.ToString()
            txtLoaiKH.Text = row.Cells("LoaiKH").Value.ToString()
        End If
    End Sub
End Class

-- form san pham
Imports System.Data.SqlClient
Public Class frmSanPham
    Dim conn As SqlConnection = New SqlConnection("Data Source=.;Initial Catalog=QLBanHang;Integrated Security=True")
    Dim dt As DataTable
    Dim da As SqlDataAdapter

    Private Sub frmSanPham_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        LoadData()
    End Sub

    Private Sub LoadData()
        da = New SqlDataAdapter("SELECT * FROM SanPham", conn)
        dt = New DataTable()
        da.Fill(dt)
        dgvSanPham.DataSource = dt
    End Sub

    Private Sub btnThem_Click(sender As Object, e As EventArgs) Handles btnThem.Click
        Dim cmd As New SqlCommand("INSERT INTO SanPham(TenSP, DonGia, SoLuongTon, DonViTinh) VALUES (@TenSP,@DonGia,@SoLuong,@DVT)", conn)
        cmd.Parameters.AddWithValue("@TenSP", txtTenSP.Text)
        cmd.Parameters.AddWithValue("@DonGia", Decimal.Parse(txtDonGia.Text))
        cmd.Parameters.AddWithValue("@SoLuong", Integer.Parse(txtSoLuong.Text))
        cmd.Parameters.AddWithValue("@DVT", txtDVT.Text)
        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub

    Private Sub btnSua_Click(sender As Object, e As EventArgs) Handles btnSua.Click
        Dim cmd As New SqlCommand("UPDATE SanPham SET TenSP=@TenSP, DonGia=@DonGia, SoLuongTon=@SoLuong, DonViTinh=@DVT WHERE MaSP=@MaSP", conn)
        cmd.Parameters.AddWithValue("@MaSP", Integer.Parse(txtMaSP.Text))
        cmd.Parameters.AddWithValue("@TenSP", txtTenSP.Text)
        cmd.Parameters.AddWithValue("@DonGia", Decimal.Parse(txtDonGia.Text))
        cmd.Parameters.AddWithValue("@SoLuong", Integer.Parse(txtSoLuong.Text))
        cmd.Parameters.AddWithValue("@DVT", txtDVT.Text)
        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub

    Private Sub btnXoa_Click(sender As Object, e As EventArgs) Handles btnXoa.Click
        Dim cmd As New SqlCommand("DELETE FROM SanPham WHERE MaSP=@MaSP", conn)
        cmd.Parameters.AddWithValue("@MaSP", Integer.Parse(txtMaSP.Text))
        conn.Open()
        cmd.ExecuteNonQuery()
        conn.Close()
        LoadData()
    End Sub
End Class

--form hoa don
Imports System.Data.SqlClient

Public Class frmHoaDon
    Dim conn As SqlConnection = New SqlConnection("Data Source=.;Initial Catalog=QLBanHang;Integrated Security=True")
    Dim dtSP As DataTable
    Dim daSP As SqlDataAdapter

    Private Sub frmHoaDon_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        LoadKhachHang()
        LoadSanPham()
    End Sub

    Private Sub LoadKhachHang()
        Dim da As New SqlDataAdapter("SELECT MaKH, TenKH FROM KhachHang", conn)
        Dim dt As New DataTable()
        da.Fill(dt)
        cboKhachHang.DataSource = dt
        cboKhachHang.DisplayMember = "TenKH"
        cboKhachHang.ValueMember = "MaKH"
    End Sub

    Private Sub LoadSanPham()
        daSP = New SqlDataAdapter("SELECT * FROM SanPham", conn)
        dtSP = New DataTable()
        daSP.Fill(dtSP)
        dgvSanPham.DataSource = dtSP
    End Sub

    Private Sub btnTaoHoaDon_Click(sender As Object, e As EventArgs) Handles btnTaoHoaDon.Click
        -- Thêm bản ghi vào bảng HoaDon trước
        Dim cmdHD As New SqlCommand("INSERT INTO HoaDon (NgayLap, MaKH, TongTien) OUTPUT INSERTED.MaHD VALUES (@NgayLap,@MaKH,@TongTien)", conn)
        cmdHD.Parameters.AddWithValue("@NgayLap", dtpNgayLap.Value)
        cmdHD.Parameters.AddWithValue("@MaKH", cboKhachHang.SelectedValue)
        cmdHD.Parameters.AddWithValue("@TongTien", Decimal.Parse(txtTongTien.Text))

        conn.Open()
        Dim newMaHD As Integer = cmdHD.ExecuteScalar()

        -- Thêm các dòng chi tiết hóa đơn
        For Each row As DataGridViewRow In dgvChiTietHoaDon.Rows
            If Not row.IsNewRow Then
                Dim cmdCT As New SqlCommand("INSERT INTO ChiTietHoaDon (MaHD, MaSP, SoLuong, DonGia) VALUES (@MaHD,@MaSP,@SoLuong,@DonGia)", conn)
                cmdCT.Parameters.AddWithValue("@MaHD", newMaHD)
                cmdCT.Parameters.AddWithValue("@MaSP", row.Cells("MaSP").Value)
                cmdCT.Parameters.AddWithValue("@SoLuong", row.Cells("SoLuong").Value)
                cmdCT.Parameters.AddWithValue("@DonGia", row.Cells("DonGia").Value)
                cmdCT.ExecuteNonQuery()
            End If
        Next

        conn.Close()
        MessageBox.Show("Tạo hóa đơn thành công!", "Thông báo", MessageBoxButtons.OK, MessageBoxIcon.Information)
    End Sub

    Private Sub dgvSanPham_CellDoubleClick(sender As Object, e As DataGridViewCellEventArgs) Handles dgvSanPham.CellDoubleClick
        -- Khi double click sản phẩm → thêm vào DataGridView ChiTietHoaDon
        If e.RowIndex >= 0 Then
            Dim spRow As DataGridViewRow = dgvSanPham.Rows(e.RowIndex)
            dgvChiTietHoaDon.Rows.Add(spRow.Cells("MaSP").Value, spRow.Cells("TenSP").Value, 1, spRow.Cells("DonGia").Value)
            TinhTongTien()
        End If
    End Sub

    Private Sub dgvChiTietHoaDon_CellValueChanged(sender As Object, e As DataGridViewCellEventArgs) Handles dgvChiTietHoaDon.CellValueChanged
        TinhTongTien()
    End Sub

    Private Sub TinhTongTien()
        Dim tong As Decimal = 0
        For Each row As DataGridViewRow In dgvChiTietHoaDon.Rows
            If Not row.IsNewRow Then
                tong += Convert.ToDecimal(row.Cells("SoLuong").Value) * Convert.ToDecimal(row.Cells("DonGia").Value)
            End If
        Next
        txtTongTien.Text = tong.ToString()
    End Sub
End Class
