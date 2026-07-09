# ConsoleJogoDeXadrez
Tabuleiro de Xadrez 8x8. Exibir as peças em suas posições iniciais, receber a entrada do Jogador, criar uma base para evoluir o jogo.
using System;

class Program
{
    static char[,] tabuleiro = new char[8, 8];

    static void Main()
    {
        InicializarTabuleiro();
        MostrarTabuleiro();

        while (true)
        {
            Console.WriteLine("Digite o movimento (ex: e2 e4): ");
            string entrada = Console.ReadLine();
            if (entrada.ToLower() == "sair") break;

            string[] partes = entrada.Split(' ');
            if (partes.Length != 2)
            {
                Console.WriteLine("Formato inválido!");
                continue;
            }

            (int linhaOrigem, int colunaOrigem) = ConverterPosicao(partes[0]);
            (int linhaDestino, int colunaDestino) = ConverterPosicao(partes[1]);

            if (MovimentoValido(linhaOrigem, colunaOrigem, linhaDestino, colunaDestino))
            {
                tabuleiro[linhaDestino, colunaDestino] = tabuleiro[linhaOrigem, colunaOrigem];
                tabuleiro[linhaOrigem, colunaOrigem] = '.';
            }
            else
            {
                Console.WriteLine("Movimento inválido!");
            }

            MostrarTabuleiro();
        }
    }

    static void InicializarTabuleiro()
    {
        // Preenche com vazio
        for (int i = 0; i < 8; i++)
            for (int j = 0; j < 8; j++)
                tabuleiro[i, j] = '.';

        // Peões
        for (int j = 0; j < 8; j++)
        {
            tabuleiro[1, j] = 'P'; // Peões brancos
            tabuleiro[6, j] = 'p'; // Peões pretos
        }

        // Torres
        tabuleiro[0, 0] = tabuleiro[0, 7] = 'T';
        tabuleiro[7, 0] = tabuleiro[7, 7] = 't';

        // Cavalos
        tabuleiro[0, 1] = tabuleiro[0, 6] = 'C';
        tabuleiro[7, 1] = tabuleiro[7, 6] = 'c';

        // Bispos
        tabuleiro[0, 2] = tabuleiro[0, 5] = 'B';
        tabuleiro[7, 2] = tabuleiro[7, 5] = 'b';

        // Rainhas
        tabuleiro[0, 3] = 'Q';
        tabuleiro[7, 3] = 'q';

        // Reis
        tabuleiro[0, 4] = 'K';
        tabuleiro[7, 4] = 'k';
    }

    static void MostrarTabuleiro()
    {
        Console.WriteLine("  a b c d e f g h");
        for (int i = 0; i < 8; i++)
        {
            Console.Write((8 - i) + " ");
            for (int j = 0; j < 8; j++)
            {
                Console.Write(tabuleiro[i, j] + " ");
            }
            Console.WriteLine((8 - i));
        }
        Console.WriteLine("  a b c d e f g h");
    }

    static (int, int) ConverterPosicao(string pos)
    {
        int coluna = pos[0] - 'a';
        int linha = 8 - int.Parse(pos[1].ToString());
        return (linha, coluna);
    }

    static bool MovimentoValido(int linhaOrigem, int colunaOrigem, int linhaDestino, int colunaDestino)
    {
        if (linhaOrigem < 0 || linhaOrigem > 7 || colunaOrigem < 0 || colunaOrigem > 7) return false;
        if (linhaDestino < 0 || linhaDestino > 7 || colunaDestino < 0 || colunaDestino > 7) return false;

        char peca = tabuleiro[linhaOrigem, colunaOrigem];
        if (peca == '.') return false;

        // Movimento simples: permite mover para qualquer casa vazia ou capturar
        return true;
    }
}
