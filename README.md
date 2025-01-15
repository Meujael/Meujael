// ==UserScript==
// @name         Preenchimento Automático de Login Atualizado
// @namespace    http://tampermonkey.net/
// @version      1.4
// @author       Marques
// @match        https://visa.vfsglobal.com/*
// @grant        none
// ==/UserScript==

(function () {
  'use strict';

  // Lista de contas
  const contas = [
    { email: "mikasamoreco@gmail.com", senha: "Cleuria2122!" },
    { email: "mikasamoreco@gmail.com", senha: "Cleuria2122!" }
  ];

  let contaAtual = contas[0]; // Conta padrão

  function preencherLogin() {
    const campoEmail = document.querySelector('input[type="email"], input[name="email"], input#email');
    const campoSenha = document.querySelector('input[type="password"], input[name="password"], input#password');

    if (campoEmail && campoSenha) {
      campoEmail.value = contaAtual.email;
      campoEmail.dispatchEvent(new Event('input', { bubbles: true }));

      campoSenha.value = contaAtual.senha;
      campoSenha.dispatchEvent(new Event('input', { bubbles: true }));

      console.log("Campos preenchidos com sucesso!");
    } else {
      console.warn("Campos não encontrados.");
    }
  }

  function alternarConta() {
    let novaConta = prompt("Escolha a conta (0 para primeira, 1 para segunda):", "0");
    if (novaConta !== null && contas[novaConta]) {
      contaAtual = contas[novaConta];
      preencherLogin();
    } else {
      alert("Conta inválida!");
    }
  }

  function criarBotao() {
    const botao = document.createElement("button");
    botao.textContent = "Preencher Login";
    botao.style.position = "fixed";
    botao.style.top = "50%";  // Centraliza verticalmente
    botao.style.left = "50%"; // Centraliza horizontalmente
    botao.style.transform = "translate(-50%, -50%)"; // Ajusta para garantir que o centro seja exato
    botao.style.padding = "10px";
    botao.style.backgroundColor = "#4CAF50";
    botao.style.color = "#fff";
    botao.style.border = "none";
    botao.style.borderRadius = "5px";
    botao.style.cursor = "pointer";
    botao.style.zIndex = "9999";

    botao.onclick = alternarConta;

    document.body.appendChild(botao);
  }

  // Tenta preencher os campos após o carregamento
  setInterval(preencherLogin, 1000);

  window.addEventListener('load', criarBotao);
})();