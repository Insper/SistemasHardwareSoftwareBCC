# Lab de Processos e Bibliotecas

No lab de Sistemas Operacionais iremos trabalhar com bibliotecas e processos para a criação de um programa similar ao `wget`. Nosso programa, chamado de `web_downloader`, fará download de páginas web usando a biblioteca `libCurl`.

## Restrições

Este exercício serve como avaliação dos conceitos vistos na disciplina. Portanto, algumas restrições serão aplicadas aos código de vocês:

- todo trabalho com arquivos deverá ser feito usando as APIs POSIX vistas em aula
- seu programa deverá compilar sem warnings usando as opções `-Wall -Wno-unused-result -Og -g -lcurl`
- você deverá usar a biblioteca `libCurl` para realizar o download das páginas. Veja sua [documentação](https://curl.se/libcurl/c/libcurl-easy.html) para aprender a usá-la
- se você usar algum trecho de código da documentação (ou de outra fonte), coloque uma atribuição em um comentário no código.

## Entrega

Você deverá colocar sua entrega na pasta `lab/lab-processos` em seu repositório de atividades, na branch principal. Não precisa soltar release/tag.

## Avaliação

O programa será avaliado de forma manual usando uma rubrica que descreve as funcionalidades implementadas. Quanto maior o número de funcionalidades maior será a nota.

### Conceito **I (zero)**

- O programa não compila
- O programa não implementa algum dos requisitos da rubrica  **D**.

### Conceito **D (4,0)**

- O programa compila com warnings.
- O programa roda na linha de comando como abaixo, salvando o resultado como `exemplo_com_pagina.html`, ou seja, ignorando `http://` e `https://`, substituindo todas barras e pontos (exceto o último) por `_`.

`$> web_downloader http://exemplo.com/pagina.html`

### Conceito **C (6,0)**

- O programa compila sem warnings.
- O programa recebe uma flag `-f` seguida pelo nome de um arquivo. Seu programa deverá ler o arquivo e fazer o download de cada url dentro do arquivo. Você pode supor que cada linha do arquivo contém exatamente uma URL. As regras para o nome do arquivo correspondente são as mesmas do item anterior.

`$> web_downloader -f lista_download.txt`

- O download de cada página é feito em um processo separado e em paralelo. Você deve observar uma diminuição grande no tempo de download de páginas pequenas.
- Ao terminar de baixar uma página, você deverá mostrar a mensagem "{url} baixada com sucesso!"

### **Conceito B (8,0)**

- O processo principal só termina depois que todos os arquivos foram baixados.
- O programa abre até `N` processos em paralelo. Se houver mais que `N` urls então os processos deverão sempre existir no máximo `N+1` processos (`N` para fazer download mais o original). Esse valor é passado pela linha de comando via flag `-N`. Se nada for passado assuma `N=4`.

### Conceito **A (10,0)**

- As mensagens de finalização de baixar uma página são mostradas sem estar embaralhadas mesmo se vários processos terminarem ao mesmo tempo.
- Ao apertar Ctrl+C o programa pergunta se o usuário deseja realmente sair. Se sim, todas as transferências são paradas e os arquivos que não foram baixados até o fim são deletados.
- Se o download falhar por alguma razão seu programa deverá mostrar a mensagem "{url} não pode ser baixada.". Nenhum arquivo deverá ser produzido neste caso.

**Obs:** Considere os conceitos como cumulativos, ou seja, cada versão deve manter as funcionalidades do conceito anterior e acrescentar novas. Ex: a versão do conceito **B** também deve funcionar com download direto passando apenas a URL (sem `-f arquivo`).

### Prazo:

[Clique aqui!](../../sobre).

### Dicas:

- Instale o pacote *libcurl4-openssl-dev* 

<div class="termy">
```console
$ sudo apt install libcurl4-openssl-dev
```
</div>

- Exemplo muuuuuito simples para comerçar:
```c
/*
Compile:
   gcc downloaderExemplo.c -o downloaderExemplo -Wall -Wno-unused-result -Og -g -lcurl
Teste:
   ./downloaderExemplo https://www.vivabonito.com.br/wp-content/uploads/2022/01/Gruta-do-Mimoso-5.jpg mimoso.jpg
Referência:
   https://curl.se/libcurl/c/simple.html
*/

#include<stdio.h>
#include<curl/curl.h>

int main(int argc, char **argv)
{   CURL *curl;
    FILE *fp;
    int resultado;

    curl = curl_easy_init();
    if(curl)
    {   
        if(argc != 3)
        {   printf("ERRO: use o downloader assim:\n\n./downloaderExemplo  URL  NOME_ARQUIVO_LOCAL\n");
            return -1;
        }
        
        // segundo parâmetro é nome do arquivo a ser gravado
        fp = fopen(argv[2], "wb");
        
        // primeiro parâmetro é URL a ser baixada
        curl_easy_setopt(curl, CURLOPT_URL, argv[1]);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, fp);
        curl_easy_setopt(curl, CURLOPT_FAILONERROR, 1L);

        // faça o download
        resultado = curl_easy_perform(curl);

        if(resultado == CURLE_OK)
            printf("Arquivo baixado corretamente\n");
        else
            printf("ERRO: %s\n", curl_easy_strerror(resultado));
        
        fclose(fp);
        curl_easy_cleanup(curl);
    }
    return 0;
}
```

- Veja outro exemplo em: *https://curl.se/libcurl/c/10-at-a-time.html*. Compile-o assim:

<div class="termy">
```console
$ gcc testeCurl.c -o testeCurl -Wall -Wno-unused-result -Og -g -lcurl
```
</div>



