# BTBuoi5
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Drawing.Text;

namespace Buoi5_BTVN
{
    public partial class Form1 : Form
    {
        private string currentFile = null;

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
           
            cmbFonts.Items.Clear();
            foreach (FontFamily font in new InstalledFontCollection().Families)
                cmbFonts.Items.Add(font.Name);

            
            int[] sizes = {8, 9, 10, 11, 12, 14, 16, 18, 20, 22, 24, 26, 28, 36, 48, 72};
            cmbSize.Items.Clear();
            foreach (int size in sizes)
                cmbSize.Items.Add(size);

         
            cmbFonts.SelectedItem = "Tahoma";
            cmbSize.SelectedItem = 14;
            richText.Font = new Font("Tahoma", 14);
        }

        private void mnuNew_Click(object sender, EventArgs e)
        {
            richText.Clear();
            cmbFonts.SelectedItem = "Tahoma";
            cmbSize.SelectedItem = 14;
            richText.Font = new Font("Tahoma", 14);
            currentFile = null;
        }

        private void mnuOpen_Click(object sender, EventArgs e)
        {
            OpenFileDialog ofd = new OpenFileDialog();
            ofd.Filter = "Rich Text Format|*.rtf|Text Files|*.txt";
            if (ofd.ShowDialog() == DialogResult.OK)
            {
                if (ofd.FileName.EndsWith(".rtf"))
                    richText.LoadFile(ofd.FileName);
                else
                    richText.Text = System.IO.File.ReadAllText(ofd.FileName);
                currentFile = ofd.FileName;
            }
        }

        private void mnuSave_Click(object sender, EventArgs e)
        {
            if (string.IsNullOrEmpty(currentFile))
            {
                SaveFileDialog sfd = new SaveFileDialog();
                sfd.Filter = "Rich Text Format|*.rtf";
                if (sfd.ShowDialog() == DialogResult.OK)
                {
                    richText.SaveFile(sfd.FileName);
                    currentFile = sfd.FileName;
                    MessageBox.Show("Lưu thành công!");
                }
            }
            else
            {
                richText.SaveFile(currentFile);
                MessageBox.Show("Lưu thành công!");
            }
        }

        private void mnuExit_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void btnBold_Click(object sender, EventArgs e)
        {
            Font currentFont = richText.SelectionFont;
            FontStyle newStyle = currentFont.Style ^ FontStyle.Bold;
            richText.SelectionFont = new Font(currentFont, newStyle);
        }

        private void btnItalic_Click(object sender, EventArgs e)
        {
            Font currentFont = richText.SelectionFont;
            FontStyle newStyle = currentFont.Style ^ FontStyle.Italic;
            richText.SelectionFont = new Font(currentFont, newStyle);
        }

        private void btnUnderline_Click(object sender, EventArgs e)
        {
            Font currentFont = richText.SelectionFont;
            FontStyle newStyle = currentFont.Style ^ FontStyle.Underline;
            richText.SelectionFont = new Font(currentFont, newStyle);
        }

        private void cmbFonts_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (cmbFonts.SelectedItem != null && cmbSize.SelectedItem != null)
                richText.SelectionFont = new Font(cmbFonts.SelectedItem.ToString(), Convert.ToInt32(cmbSize.SelectedItem));
        }

        private void cmbSize_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (cmbFonts.SelectedItem != null && cmbSize.SelectedItem != null)
                richText.SelectionFont = new Font(cmbFonts.SelectedItem.ToString(), Convert.ToInt32(cmbSize.SelectedItem));
        }

        private void richText_TextChanged(object sender, EventArgs e)
        {
            int wordCount = richText.Text.Split(new char[] {' ', '\n', '\r'}, StringSplitOptions.RemoveEmptyEntries).Length;
            lblWordCount.Text = $"Tổng số từ: {wordCount}";
        }

        private void mnuFont_Click(object sender, EventArgs e)
        {
            FontDialog fontDlg = new FontDialog();
            fontDlg.ShowColor = true;
            fontDlg.ShowApply = true;
            fontDlg.ShowEffects = true;
            fontDlg.ShowHelp = true;
            if (fontDlg.ShowDialog() != DialogResult.Cancel)
            {
                richText.ForeColor = fontDlg.Color;
                richText.Font = fontDlg.Font;
            }
        }
    }
}
