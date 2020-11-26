# Soft152-Assignment
Applications that takes datab

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WordStatsProject
{
    public partial class WordSatsForm : Form
    {
        private string fileName;

        private WordStatistics wordStatistics;

        public WordSatsForm()
        {
            InitializeComponent();

            wordStatistics = new WordStatistics();
        }

        private void openToolStripMenuItem_Click(object sender, EventArgs e)
        {
            if (openFileDialog.ShowDialog() != DialogResult.Cancel)
            {
                fileName = openFileDialog.FileName;

                Initialse();

                LoadFile();

                DisplayStats();
            }
        }

        private void LoadFile()
        {
            string lineFromFile;
            var column1 = new List<string>();
            try
            {
                using (StreamReader reader = new StreamReader(fileName))
                {
                    wordStatistics.InitialiseStatistics();
                    while (!reader.EndOfStream)
                    {
                        lineFromFile = reader.ReadLine();
                      
                        ProcesslineFromFile(lineFromFile);
                    }
                }  // end using
            }
            catch (FileNotFoundException ex)
            {
                MessageBox.Show("File Not Found");
            }
        }  // end LoadFile()

        private void ProcesslineFromFile(string inputText)
        {
            wordStatistics.ProcessLineofText(inputText);

        }  // end processlineFromfile


        private void Initialse()
        {
            string pattern;

            InitialiseLabels();

            wordStatistics.InitialiseStatistics();

            pattern = patternTextBox.Text;

            wordStatistics.PatternToMatch = pattern;

            wordStatistics.IgnoreCase = caseCheckBox.Enabled;

        }

        private void InitialiseLabels()  // This code automatically delcare has zero
        {
            AverageeGradeFirstattLabel.Text = "0";
            NumberLine.Text = "0";
            TotalNumAttempts.Text = "0";
            AverageGradeLabel.Text = "0";
            AverageTimetoCompleteLabel.Text = "0";
            AverageTimeNon1stAttemptsLabel.Text = "0";

        }

        private void DisplayStats() // All the information display through the label going through this code. 
        {
            AverageeGradeFirstattLabel.Text = wordStatistics.AverageGradeofFirstAtt.ToString() + "%";
            NumberLine.Text = wordStatistics.NumberLines.ToString();
            TotalNumAttempts.Text = wordStatistics.TotalNumAttempts.ToString();
            AverageGradeLabel.Text = wordStatistics.NumberPatternMatches.ToString() + "%";
            AverageTimetoCompleteLabel.Text = wordStatistics.AverageTimetoComplete.ToString();
            AverageTimeNon1stAttemptsLabel.Text = wordStatistics.AverageTimeNon1stAttempts.ToString();
        }

        private void WordSatsForm_Load(object sender, EventArgs e)
        {

        }

        private void NumberOfFirstAttempts_Click(object sender, EventArgs e)
        {

        }

        private void AverageGradeLabel_Click(object sender, EventArgs e)
        {

        }
    }
}
