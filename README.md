# Leitor, Ordenador e Escritor de Arquivos (MIPS Assembly)

Este programa em Assembly MIPS (simulador MARS) tem a finalidade de:

- Ler uma lista de números inteiros de um arquivo de texto.
- Ordenar esses números em ordem crescente (utilizando o algoritmo **Bubble Sort**).
- Escrever a lista ordenada em um novo arquivo de texto.

---

## 1. Pré-requisitos

- **Simulador MARS (MIPS Assembler and Runtime Simulator)**:  
  O código é projetado para este ambiente.

---

## 2. Configuração e Uso

### 2.1. Ajuste dos Caminhos (Obrigatório)

Você **DEVE** ajustar os caminhos para os arquivos de entrada e saída na seção `.data` para corresponderem ao seu sistema:

```
.data
    filename_input:  .asciiz "C:\\...\\caminho\\para\\lista.txt"
    filename_output: .asciiz "C:\\...\\caminho\\para\\lista_ordenada.txt"
```

---

## 2.2 Limites de Alocação

- A diretiva `.eqv size 400` é usada no código para definir o limite superior do loop de ordenação, representando **400 bytes** ou **100 inteiros**.
- Capacidade Máxima: O vetor de inteiros (`vetor_int`) está alocado com **800 bytes**, permitindo armazenar até **200 inteiros** (cada inteiro ocupa 4 bytes).

> Se precisar armazenar mais de 200 números, você deve ajustar simultaneamente:
> - O valor da constante `size` (para o número de bytes).  
> - A diretiva `.space` em `vetor_int`.

---

## 2.3 Execução

1. Carregue o código no **MARS**.
2. Verifique e ajuste os caminhos dos arquivos.
3. Execute o programa: `Run > Assemble -> Run > Go`.

> Ao finalizar, um novo arquivo (`lista_ordenada.txt`) será criado no caminho especificado com a lista de números ordenados.

---

## 3. Fluxo do Programa (Resumo)

| Rótulo Principal/Função       | Etapa                     | Descrição |
|-------------------------------|--------------------------|-----------|
| `conversao_strint`            | Leitura e Parsing        | Converte a string de entrada (`input_buffer`) em números inteiros, tratando sinais negativos e vírgulas, armazenando-os em `vetor_int`. |
| `ordenacao_i` / `ordenacao_j` | Ordenação                | Implementa o **Bubble Sort** para ordenar o vetor de inteiros (`vetor_int`) em ordem crescente. |
| `escrita_numeros`             | Conversão e Formatação   | Converte os números ordenados de volta para caracteres ASCII e os formata em uma string com vírgulas (`vetor_str`). |
| `escrita`                     | Saída de Dados           | Abre o arquivo de saída e escreve o conteúdo formatado de `vetor_str` nele, e fecha ambos os arquivos. |

---

## Exportar para as Planilhas

---

## 4. Alocação de Registradores (Principais)

| Registrador       | Uso Principal |
|------------------|---------------|
| `$t0`             | Descritor de Arquivo (FD) |
| `$t3`             | Valor do Número em Processamento (Conversão ou comparação/troca) |
| `$t4`             | Índice no Vetor de Inteiros (`vetor_int`) / Índice auxiliar (`j`) |
| `$t6`             | Sinal (+1 ou -1) / Índice principal (`i`) do Bubble Sort |
| `$v0, $a0-$a2`   | Argumentos e códigos para `syscall` (I/O) |
