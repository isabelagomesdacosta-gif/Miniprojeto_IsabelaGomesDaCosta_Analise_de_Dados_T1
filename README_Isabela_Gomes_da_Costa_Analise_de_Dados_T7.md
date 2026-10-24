# Instruções de execução — Isabela Gomes da Costa

Mini-projeto de Análise Exploratória de Dados do Varejo.

## Executar no VS Code

1. Mantenha `mini_projeto_analise_de_dados_com_python.ipynb`, `base_varejo.csv` e `requirements.txt` na mesma pasta e abra essa pasta no VS Code.
2. Instale as extensões Python e Jupyter, da Microsoft.
3. No terminal, execute:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Abra o notebook e selecione o mesmo ambiente Python como kernel.
5. Reinicie o kernel e use **Executar Tudo**, sem pular células. Se uma célula apresentar erro, resolva antes de continuar.

O notebook apresenta as análises e o boxplot na saída das células. Não execute o `.ipynb` diretamente com o comando `python`.

## Resultados

O bloco final exporta `df_limpo.csv` e a pasta `resultados`, com nulos originais, estatísticas e modas do número de filhos, resumos por sexo informado e categoria e relatório final. Os arquivos são gravados na pasta de trabalho do kernel e substituídos em novas execuções.

