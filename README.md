using System;

class Program
{
    static char[,] board = {
        { '1', '2', '3' },
        { '4', '5', '6' },
        { '7', '8', '9' }
    };

    static int turns = 0;
    static char currentPlayer = 'X';

    static void Main()
    {
        bool gameEnded = false;

        while (!gameEnded)
        {
            Console.Clear();
            DrawBoard();

            Console.WriteLine($"Jogador {currentPlayer}, escolha uma posição (1-9): ");
            string input = Console.ReadLine();

            if (!int.TryParse(input, out int pos) || pos < 1 || pos > 9)
                continue;

            int row = (pos - 1) / 3;
            int col = (pos - 1) % 3;

            if (board[row, col] == 'X' || board[row, col] == 'O') continue;

            board[row, col] = currentPlayer;
            turns++;

            if (CheckWinner())
            {
                Console.Clear();
                DrawBoard();
                Console.WriteLine($"Jogador {currentPlayer} venceu!");
                gameEnded = true;
            }
            else if (turns == 9)
            {
                Console.Clear();
                DrawBoard();
                Console.WriteLine("Empate!");
                gameEnded = true;
            }
            else
            {
                currentPlayer = currentPlayer == 'X' ? 'O' : 'X';
            }
        }
    }

    static void DrawBoard()
    {
        Console.WriteLine("Jogo da Velha");
        for (int i = 0; i < 3; i++)
        {
            Console.WriteLine(" " + board[i, 0] + " | " + board[i, 1] + " | " + board[i, 2]);
            if (i < 2)
                Console.WriteLine("---+---+---");
        }
    }

    static bool CheckWinner()
    {
        // Linhas e colunas
        for (int i = 0; i < 3; i++)
        {
            if ((board[i, 0] == currentPlayer &&
                 board[i, 1] == currentPlayer &&
                 board[i, 2] == currentPlayer) ||
                (board[0, i] == currentPlayer &&
                 board[1, i] == currentPlayer &&
                 board[2, i] == currentPlayer))
                return true;
        }
        // Diagonais
        if ((board[0,0] == currentPlayer &&
             board[1,1] == currentPlayer &&
             board[2,2] == currentPlayer) ||
            (board[0,2] == currentPlayer &&
             board[1,1] == currentPlayer &&
             board[2,0] == currentPlayer))
            return true;

        return false;
    }
}
         
