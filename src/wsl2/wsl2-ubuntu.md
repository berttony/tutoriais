<!--
Arquivo: wsl2-ubuntu.md
Descrição: Guia para instalar e configurar o Ubuntu no WSL2 (Windows Subsystem for Linux 2), incluindo dicas para garantir o funcionamento correto e comandos úteis para gerenciamento de distribuições.
-->

# Guia de Instalação e Configuração do Ubuntu no WSL2

Este tutorial explica como instalar o Ubuntu no WSL2, ativar a virtualização e gerenciar distribuições Linux no Windows.

1. **Ative a Virtualização de Hardware**
   - Reinicie o PC e acesse o BIOS/UEFI (geralmente pressionando Del, Esc, F1, F2 ou F4).
   - Procure por uma opção chamada Intel VT-x, AMD-V, Hyper-V ou SVM e ative-a.
   - *Explicação:* A virtualização é necessária para o WSL2 funcionar corretamente.

2. **Instale o Ubuntu 24.04.1 LTS pela Microsoft Store**
   - *Explicação:* O Ubuntu será sua distribuição Linux dentro do Windows.

3. **Verifique se o WSL está na versão 2**
   - Comando: `wsl --set-default-version 2`
   - *Explicação:* O WSL2 oferece melhor desempenho e compatibilidade com Docker e outras ferramentas.

---

## Comandos Úteis no PowerShell (como administrador)

- `wsl --install`
  - Instala o WSL e uma distribuição padrão.
- `wsl --update`
  - Atualiza o WSL para a versão mais recente.
- `wsl --set-default-version 2`
  - Define o WSL2 como padrão para novas distribuições.
- Após reiniciar, instale uma distribuição Linux pela Microsoft Store.

---

## Gerenciamento de Distribuições

- `wsl --list --verbose`
  - Lista as distribuições instaladas e suas versões.
- `wsl --list --online`
  - Lista distribuições disponíveis para instalação.