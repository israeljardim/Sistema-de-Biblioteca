# Sistema-de-Biblioteca
Um sistema de biblioteca é uma plataforma que facilita a gestão de uma biblioteca. O sistema mantém um catálogo organizado, registra a disponibilidade de materiais e controla as datas de empréstimo e devolução. Dessa forma, os usuários podem acessar a coleção e manter um registro de suas atividades na biblioteca.

## Classe Bibliotecário

`bibliotecario.cpp`

```c++

#include "Bibliotecario.hpp"
#include "Bibliotecario.hpp"
#include "ControleEmprestimo.hpp"  
#include "Pesquisa.hpp"  

void Bibliotecario::cadastrarLivro(ControleAcervo& acervo) {
    std::string titulo, autor, edicao, editora, sinopse, categoria;
    int numPaginas;

    std::cout << "Digite o título do livro: ";
    std::getline(std::cin, titulo);

    std::cout << "Digite o autor do livro: ";
    std::getline(std::cin, autor);

    std::cout << "Digite a edição do livro: ";
    std::getline(std::cin, edicao);

    std::cout << "Digite a editora do livro: ";
    std::getline(std::cin, editora);

    std::cout << "Digite a sinopse do livro: ";
    std::getline(std::cin, sinopse);

    std::cout << "Digite o número de páginas do livro: ";
    std::cin >> numPaginas;

    std::cout << "Digite a categoria do livro: ";
    std::cin.ignore(); // Para limpar o buffer do teclado antes de pegar a categoria
    std::getline(std::cin, categoria);

    Livro novoLivro(titulo, autor, edicao, editora, sinopse, numPaginas, categoria);
    acervo.armazenarLivro(novoLivro);

    std::cout << "Livro cadastrado com sucesso!" << std::endl;
}

void Bibliotecario::cadastrarUsuario(Usuario& usuario) {
}

void Bibliotecario::realizarEmprestimo(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario) {
    // Supondo que você tenha um livro disponível no acervo
    Livro livro = acervo.obterLivroDisponivel();
    
    // Chame a função registrarEmprestimo com o livro obtido
    controleEmprestimo.registrarEmprestimo(livro, usuario, acervo);    
	
	// Supondo que o usuário já esteja cadastrado
    std::string codigoLivro;
    std::cout << "Digite o código do livro a ser emprestado: ";
    std::cin >> codigoLivro;

    // Encontrar o livro no acervo
    Livro livro = acervo.buscarLivroPorCodigo(codigoLivro);

    // Realizar empréstimo
    controleEmprestimo.registrarEmprestimo(acervo, usuario, livro);

    std::cout << "Empréstimo realizado com sucesso!" << std::endl;
}
void Bibliotecario::realizarDevolucao(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario) {
    // Supondo que você tenha um livro emprestado pelo usuário
    Livro livro = controleEmprestimo.obterLivroEmprestado(usuario);
    
    @param Chame a função registrarDevolucao com o livro obtido
    controleEmprestimo.registrarDevolucao(livro, usuario, acervo);

    @param Supondo que o usuário já tenha livros emprestados
    std::string codigoLivro;
    std::cout << "Digite o código do livro a ser devolvido: ";
    std::cin >> codigoLivro;

    @param Encontrar o livro no acervo
    Livro livro = acervo.buscarLivroPorCodigo(codigoLivro);

    @param Realizar devolução
    controleEmprestimo.registrarDevolucao(acervo, usuario, livro);

    std::cout << "Devolução realizada com sucesso!" << std::endl;
}
```

`bibliotecario.hpp`

```c++

#ifndef BIBLIOTECARIO_HPP
#define BIBLIOTECARIO_HPP

#include <iostream>
#include "ControleAcervo.hpp"
#include "ControleEmprestimo.hpp" 
#include "Pesquisa.hpp"  
#include "Usuario.hpp"

class Bibliotecario {
public:
    void cadastrarLivro(ControleAcervo& acervo);
    void atualizarInformacoesLivro(ControleAcervo& acervo, Livro& livro);
    void cadastrarUsuario(Usuario& usuario);
    void atualizarInformacoesUsuario(Usuario& usuario);
    void emitirCarteiraIdentificacao(Usuario& usuario);
	void realizarEmprestimo(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario);
    void realizarDevolucao(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario);
    void realizarPesquisa(ControleAcervo& acervo, Usuario& usuario, Pesquisa& pesquisa);
};

#endif // BIBLIOTECARIO_HPP

```

## Classe Controle de Acervo

`ControleAcervo.cpp`

```c++
    // Exemplo simples: imprimir os livros agrupados por autor
    std::map<std::string, std::vector<Livro>> livrosPorAutor;

    for (const auto& livro : livros) {
        livrosPorAutor[livro.getAutor()].push_back(livro);
    }

    for (const auto& [autor, livrosAutor] : livrosPorAutor) {
        std::cout << "Livros do autor " << autor << ":\n";
        for (const auto& livro : livrosAutor) {
            livro.mostrarInformacoes();
            std::cout << "----------\n";
        }
    }
}

void ControleAcervo::registrarEmprestimo(const Livro& livro, const Usuario& usuario) {
     // Verificar a disponibilidade do livro
    if (!livro.estaDisponivel()) {
        std::cout << "O livro não está disponível para empréstimo.\n";
        return;
    }
}

void ControleAcervo::registrarDevolucao(const Livro& livro, const Usuario& usuario) {
    // Aqui você pode verificar se o livro foi emprestado pelo usuário,
    // registrar a devolução, atualizar o estado do livro e registrar a devolução no histórico do usuário, etc.

    // Exemplo simples: marcar o livro como disponível
    livro.marcarComoDisponivel();

    // Exemplo simples: exibir mensagem de sucesso
    std::cout << "Devolução registrada com sucesso.\n";
}

void ControleAcervo::realizarPesquisaPorTitulo(const std::string& titulo) const {
    for (const auto& livro : livros) {
        if (livro.getTitulo() == titulo) {
            // Exibir informações do livro encontrado
            livro.mostrarInformacoes();
        }
    }
}

void ControleAcervo::realizarPesquisaPorAutor(const std::string& autor) const {
    for (const auto& livro : livros) {
        if (livro.getAutor() == autor) {
            // Exibir informações do livro encontrado
            livro.mostrarInformacoes();
        }
    }
}
```

`ControleAcervo.hpp`

```c++
#ifndef CONTROLEACERVO_HPP
#define CONTROLEACERVO_HPP

#include <iostream>
#include <vector>
#include <algorithm>
#include "Livro.hpp"
#include "Usuario.hpp"

class ControleAcervo {
private:
    std::vector<Livro> livros;

public:
    // Métodos relacionados ao acervo de livros
    void armazenarLivro(const Livro& livro);
    Livro buscarLivroPorCodigo(const std::string& codigo) const;
    void agruparPorGenero() const;
    void agruparPorAutor() const;

    // Métodos relacionados ao controle de empréstimo
    void registrarEmprestimo(const Livro& livro, const Usuario& usuario);
    void registrarDevolucao(const Livro& livro, const Usuario& usuario);

    // Métodos para pesquisa
    void realizarPesquisaPorTitulo(const std::string& titulo) const;
    void realizarPesquisaPorAutor(const std::string& autor) const;
};

#endif // CONTROLEACERVO_HPP
```
