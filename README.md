# Desktop-App
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data;
using System.Data.SqlClient;
using System.Configuration;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {

        SqlConnection con;
        SqlDataAdapter Adpt;
        DataTable dt = new DataTable();
        public Form1()
        {
            InitializeComponent();
            SqlConnection con = new SqlConnection();
            con.ConnectionString = "Data Source=.;Initial Catalog=ITI;Integrated Security=True";
            SqlCommand cmd = new SqlCommand("select * from instructor", con);
            SqlDataAdapter Adpt = new SqlDataAdapter();
            Adpt.SelectCommand = cmd;
            Adpt.Fill(dt);
            grid.DataSource = dt;
        }

        private void button1_Click(object sender, EventArgs e)
        {
            DataRow Dr = dt.NewRow();
            Dr["ins_id"] = int.Parse(txt_id.Text);
            Dr["ins_Name"] = txt_name.Text;
            Dr["ins_degree"] = txt_degree.Text;
            Dr["salary"] = double.Parse(txt_sal.Text);
            Dr["dept_id"] = int.Parse(txt_deptid.Text);
            dt.Rows.Add(Dr);
            txt_degree.Text = txt_deptid.Text = txt_id.Text = txt_name.Text = txt_sal.Text = " "; 

        }

        private void grid_RowHeaderMouseDoubleClick(object sender, DataGridViewCellMouseEventArgs e)
        {
            txt_id.Text =  grid.SelectedRows[0].Cells[0].Value.ToString();
            txt_name.Text = grid.SelectedRows[0].Cells[1].Value.ToString();
            txt_degree.Text = grid.SelectedRows[0].Cells[2].Value.ToString();
            txt_sal.Text = grid.SelectedRows[0].Cells[3].Value.ToString();
            txt_deptid.Text = grid.SelectedRows[0].Cells[4].Value.ToString();
        }

        private void button3_Click(object sender, EventArgs e)
        {
            
           // int id = int.Parse(txt_id.Text);
            
            foreach (DataRow item in dt.Rows)
            {
                if (txt_id.Text == item["ins_id"].ToString()) 
                {
                    grid.SelectedRows[0].Cells[0].Value = int.Parse(txt_id.Text);
                    grid.SelectedRows[0].Cells[1].Value = txt_name.Text;
                    grid.SelectedRows[0].Cells[2].Value = txt_degree.Text;
                    grid.SelectedRows[0].Cells[3].Value =  double.Parse(txt_sal.Text);
                    grid.SelectedRows[0].Cells[4].Value = txt_deptid.Text;
                }
            }
            txt_degree.Text = txt_deptid.Text = txt_id.Text = txt_name.Text = txt_sal.Text = " ";

        }

        private void button2_Click(object sender, EventArgs e)
        {
            grid.Rows.RemoveAt(grid.SelectedRows[0].Index);
         }

        private void Btn_search_Click(object sender, EventArgs e)
        {
            try
            {
                DataView dtv = new DataView(dt);
                dtv.RowFilter = "ins_id = " + txt_search.Text + "";
                grid.DataSource = dtv;
            }
            catch 
            {
                grid.DataSource = dt;
            }
        }

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            SqlConnection con2 = new SqlConnection();
            con2.ConnectionString = "Data Source=.;Initial Catalog=ITI;Integrated Security=True";
            SqlCommand insert = new SqlCommand("insert into instructor values(@Ins_Id,@Ins_Name,@Ins_Degree,@Salary,@Dept_Id)", con2);
            insert.Parameters.Add("Ins_Id", SqlDbType.Int, 4, "Ins_Id");
            insert.Parameters.Add("Ins_Name", SqlDbType.VarChar, 50, "Ins_Name");
            insert.Parameters.Add("Ins_Degree", SqlDbType.VarChar, 50, "Ins_Degree");
            insert.Parameters.Add("Salary", SqlDbType.Int, 4, "Salary");
            insert.Parameters.Add("Dept_Id", SqlDbType.Int, 4, "Dept_Id");

            SqlCommand delete = new SqlCommand("delete from instructor where @Ins_Id =Ins_Id", con2);
            delete.Parameters.Add("id", SqlDbType.Int, 4, "Ins_Id");

            SqlCommand update =
                new SqlCommand("update instructor set Ins_Id=@Ins_Id , Ins_Name=@Ins_Name , Ins_Degree=@Ins_Degree, Salary = @Salary , Dept_Id = @Dept_Id ", con2);
            update.Parameters.Add("Ins_Id", SqlDbType.Int, 4, "Ins_Id");
            update.Parameters.Add("Ins_Name", SqlDbType.VarChar, 50, "Ins_Name");
            update.Parameters.Add("Ins_Degree", SqlDbType.VarChar, 50, "Ins_Degree");
            update.Parameters.Add("Salary", SqlDbType.Int, 4, "Salary");
            update.Parameters.Add("Dept_Id", SqlDbType.Int, 4, "Dept_Id");

            SqlDataAdapter DAR = new SqlDataAdapter();
            DAR.InsertCommand = insert;
            DAR.DeleteCommand = delete;
            DAR.UpdateCommand = update;
            DAR.Update(dt);
            MessageBox.Show("Saved Data", "saved Data");
        }
    }
}

