# Sudoku-Solver C#

This program allows the user to enter some off the numbers that are known on the sudoku board, 
the program then fills in the rest of the numbers by the logic of the game. No same numbers on the same row, column or in the same box.


    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    
    namespace sudoku
    {
        public partial class Form1 : Form
        {
            private TextBox[,] cells = new TextBox[9, 9];
    
            public Form1()
            {
                InitializeComponent();
                createCells();
               
            }
          
            private void createCells()
            {
                for (int x = 0; x < 9; x++)
                {
                    for (int y = 0; y < 9; y++)
                    {
                        cells[x, y] = new TextBox();
                        cells[x, y].Font = new Font(SystemFonts.DefaultFont.FontFamily, 20);
                        cells[x, y].Size = new Size(40, 40);
                        cells[x, y].ForeColor = SystemColors.ControlDarkDark;
                        cells[x, y].Location = new Point(x * 40, y * 40);
                        cells[x, y].BackColor = ((x / 3) + (y / 3)) % 2 == 0 ? SystemColors.Control : Color.LightGray;
                        cells[x, y].TextAlign = HorizontalAlignment.Center;
                        panel1.Controls.Add(cells[x, y]);
    
                    }
                }
            }
            
            private void Form1_Load(object sender, EventArgs e)
            {
    
            }
    
            private void Solvebutton_Click(object sender, EventArgs e)
            {
                int[,] board = new int[9, 9];
    
                for (int x = 0; x < 9; x++)
                {
                    for (int y = 0; y < 9; y++)
                    {
                        int value;
                        if (int.TryParse(cells[x,y].Text, out value))
                        {
                            board[x,y] = value;
                        }
                        else
                        {
                            board[x,y] = 0;
                        }
                    }
                }
    
                if (Solvesudoku(board))
                {
                    for (int x = 0; x < 9; x++)
                    {
                        for (int y = 0; y < 9; y++)
                        {
                            cells[x, y].Text = board[x, y] != 0 ? board[x, y].ToString() : "";
                        }
                    }
    
                }
                else
                {
                    MessageBox.Show("NO Solution");
                
                }
    
                
            }
            private bool Solvesudoku(int[,]board)
            {
                
                for (int row = 0; row < 9; row++)
                    {
                        for (int col = 0; col < 9; col++)
                        {
                            if (board[row,col] == 0)
                            {
                                for (int num = 1; num <=9; num++)
                                {
                                    if (IsSafe(board, row, col, num))
                                    {
                                        board[row, col] = num;
                                        if (Solvesudoku(board))
                                        {
                                            return true;
                                        }
                                        board[row, col] = 0;
                                    }
                                }
                                return false;
                            }
                        }
                    }
                    return true;
            }
    
    
            private bool IsSafe(int[,] board, int row, int col, int num)
            {
                for (int x = 0; x < 9; x++)
                {
                    if (board[row,x] == num || board[x,col] == num || board[row - row % 3 + x / 3, col - col % 3 + x % 3] == num)
                    {
                        return false;
                    }
    
                }
                return true;
    
    
            }
    
            private void clear_btn_Click(object sender, EventArgs e)
            {
                for (int x = 0; x < 9; x++)
                {
                    for (int y = 0; y < 9; y++)
                    {
                        cells[x, y].Text = "";
                    }
                }
            }
        }
    }
