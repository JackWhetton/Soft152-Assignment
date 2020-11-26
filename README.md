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


Part 2

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WordStatsProject
{
    class WordStatistics
    {
        public int NumberLines { private set; get; } // need to find a way to subtract the the first row /// done

        public int TotalNumAttempts { private set; get; } // need to add up all the attempets made throught the quiz 

        public double AverageGradeofFirstAtt { private set; get; }

        public double NumberPatternMatches { private set; get; }

        public string PatternToMatch { set; get; }

        public bool IgnoreCase { set; private get; }

        public string AverageTimetoComplete { private set; get; } // precentage for all the time 

        public string AverageTimeNon1stAttempts { private set; get; } // percentage for only firstAttempts

        public double TotalFirstAttGrade { private set; get; } // This help calculate the Total first Grade

        public double TotalAllAttGrade { private set; get; }

        public int TotalFirstAttSeconds { private set; get; }

        public double AverageFirstAttSeconds { private set; get; }

        public int TotalAllSeconds { private set; get; }

        public double AverageAllSeconds { private set; get; }

        public WordStatistics()
        {
            NumberLines = 0;

            TotalNumAttempts = -1;

            AverageGradeofFirstAtt = 0;

            PatternToMatch = "a";

            NumberPatternMatches = 0;

            AverageTimetoComplete = "0";

            AverageTimeNon1stAttempts = "0";

            TotalFirstAttGrade = 0;

            TotalAllAttGrade = 0;

            TotalFirstAttSeconds = 0;

            AverageFirstAttSeconds = 0;

            TotalAllSeconds = 0;

            AverageAllSeconds = 0;

        }

        public void InitialiseStatistics()
        {
            NumberLines = 0;

            TotalNumAttempts = -1;

            AverageGradeofFirstAtt = 0;

            NumberPatternMatches = 0;

            AverageTimetoComplete = "0";

            AverageTimeNon1stAttempts = "0";

            TotalFirstAttGrade = 0;

            TotalAllAttGrade = 0;

            TotalFirstAttSeconds = 0;

            AverageFirstAttSeconds = 0;

            TotalAllSeconds = 0;

            AverageAllSeconds = 0;
        }



        // this code is used to process each line of data in the file 
        public void ProcessLineofText(string lineOftext) 
        {
            string[] wordsInText;
            string[] wordsWithin;

            char[] delimiter = { ',' };
            char[] delimiter2 = { ' ' };

            string[] WordsIntText = lineOftext.Split(',');

            double temp = 0;

            wordsInText = lineOftext.Split(delimiter, StringSplitOptions.RemoveEmptyEntries);
            // The below code work only the first number of attempts
            // by only looking for First attempts with the number one Attempt number column.
            if (wordsInText[2] == "1")
            {
                string temptime = wordsInText[3];

                wordsWithin = temptime.Split(delimiter2, StringSplitOptions.RemoveEmptyEntries);

                #region Grab Data from time taken

                if (wordsWithin[1] == "days")
                {
                    int tempdays;
                    int.TryParse(wordsWithin[0], out tempdays);
                    TotalFirstAttSeconds += (86400 * tempdays);
                }
                else if (wordsWithin[1] == "hours")
                {
                    int temphours;
                    int.TryParse(wordsWithin[0], out temphours);
                    TotalFirstAttSeconds += (3600 * temphours);
                }
                else if (wordsWithin[1] == "mins")
                {
                    int tempmins;
                    int.TryParse(wordsWithin[0], out tempmins);
                    TotalFirstAttSeconds += (60 * tempmins);
                }
                else if (wordsWithin[1] == "secs")
                {
                    int tempseconds;
                    int.TryParse(wordsWithin[0], out tempseconds);
                    TotalFirstAttSeconds += tempseconds;
                }
                else
                {
                    //do nothing.
                }

                if (wordsWithin.Length > 2)
                {
                    if (wordsWithin[3] == "days")
                    {
                        int tempdays;
                        int.TryParse(wordsWithin[2], out tempdays);
                        TotalFirstAttSeconds += (86400 * tempdays);
                    }
                    else if (wordsWithin[3] == "hours")
                    {
                        int temphours;
                        int.TryParse(wordsWithin[2], out temphours);
                        TotalFirstAttSeconds += (3600 * temphours);
                    }
                    else if (wordsWithin[3] == "mins")
                    {
                        int tempmins;
                        int.TryParse(wordsWithin[2], out tempmins);
                        TotalFirstAttSeconds += (60 * tempmins);
                    }
                    else if (wordsWithin[3] == "secs")
                    {
                        int tempseconds;
                        int.TryParse(wordsWithin[2], out tempseconds);
                        TotalFirstAttSeconds += tempseconds;
                    }
                    else
                    {
                        //do nothing.
                    }
                }

                #endregion

                NumberLines++;

                AverageFirstAttSeconds = TotalFirstAttSeconds / NumberLines;

                AverageFirstAttSeconds = Math.Round(AverageFirstAttSeconds/60, 2);

                AverageTimetoComplete = AverageFirstAttSeconds + " Minutes.";

                
                ///if the user opens another file to look at other results currently it will add on top of prefers answer
                ///i need to find a way to clear the label every time a new file is opened.
                double.TryParse(wordsInText[4], out temp);

                TotalFirstAttGrade += temp;

                AverageGradeofFirstAtt = (10*(TotalFirstAttGrade / NumberLines));

                AverageGradeofFirstAtt = Math.Round(AverageGradeofFirstAtt, 2);


            }

            string temptimeall = wordsInText[3];

            wordsWithin = temptimeall.Split(delimiter2, StringSplitOptions.RemoveEmptyEntries);

            #region Grab Data from time taken

            if (wordsWithin[1] == "days")
            {
                int tempdays;
                int.TryParse(wordsWithin[0], out tempdays);
                TotalAllSeconds += (86400 * tempdays);
            }
            else if (wordsWithin[1] == "hours")
            {
                int temphours;
                int.TryParse(wordsWithin[0], out temphours);
                TotalAllSeconds += (3600 * temphours);
            }
            else if (wordsWithin[1] == "mins")
            {
                int tempmins;
                int.TryParse(wordsWithin[0], out tempmins);
                TotalAllSeconds += (60 * tempmins);
            }
            else if (wordsWithin[1] == "secs")
            {
                int tempseconds;
                int.TryParse(wordsWithin[0], out tempseconds);
                TotalAllSeconds += tempseconds;
            }
            else
            {
                //do nothing.
            }

            if (wordsWithin.Length > 2)
            {
                if (wordsWithin[3] == "days")
                {
                    int tempdays;
                    int.TryParse(wordsWithin[2], out tempdays);
                    TotalAllSeconds += (86400 * tempdays);
                }
                else if (wordsWithin[3] == "hours")
                {
                    int temphours;
                    int.TryParse(wordsWithin[2], out temphours);
                    TotalAllSeconds += (3600 * temphours);
                }
                else if (wordsWithin[3] == "mins")
                {
                    int tempmins;
                    int.TryParse(wordsWithin[2], out tempmins);
                    TotalAllSeconds += (60 * tempmins);
                }
                else if (wordsWithin[3] == "secs")
                {
                    int tempseconds;
                    int.TryParse(wordsWithin[2], out tempseconds);
                    TotalAllSeconds += tempseconds;
                }
                else
                {
                    //do nothing.
                }
            }

            #endregion

            //Total number of attempts calcuated by subracting 1 because of the header line.
            TotalNumAttempts++;

            if (TotalNumAttempts <= 0)
            {
                AverageAllSeconds = TotalAllSeconds / 1;
            }
            else
            {
                AverageAllSeconds = TotalAllSeconds / TotalNumAttempts;
            }


            AverageAllSeconds = Math.Round(AverageAllSeconds / 60, 2);

            AverageTimeNon1stAttempts = AverageAllSeconds + " Minutes.";

            
            double temp2;
            double.TryParse(wordsInText[4], out temp2);
            TotalAllAttGrade += temp2;

            NumberPatternMatches = (10*(TotalAllAttGrade / TotalNumAttempts));
            NumberPatternMatches = Math.Round(NumberPatternMatches, 2);


        }


        private void TotalCharacterPattern(string word) // this is a further code on average grade all attempets
        {

            if (IgnoreCase)
            {
                word = word.ToUpper();
                PatternToMatch = PatternToMatch.ToUpper();
            }

            if (word.Contains(PatternToMatch))
            {
                NumberPatternMatches++;
            }
        }

    }
}










