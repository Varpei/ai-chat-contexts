## Objetivo da Sessão
Desenvolver e manter um sistema de gerenciamento de estado (Context Hub) para interações com LLMs (Gemini, Claude, Copilot) visando evitar alucinações e perda de contexto. 

## Stack Tecnológica e Arquitetura
- **Ambiente:** Linux Mint / Pop!_OS via WSL no Windows 11.
- **CLI:** Script Python (`ctx-sync`) operando comandos Git no backend.
- **Armazenamento:** Repositório local `~/ai-chat-contexts` sincronizado com GitHub (`Varpei/ai-chat-contexts`).
- **Interface:** MkDocs com Material Theme rodando no GitHub Pages via GitHub Actions (deploy automatizado em todo push).
- **Metodologia:** Anti-Vibe Coding (determinismo, sem abstrações).

## Paradigma de Operação
- 1 Chat de IA = 1 Diretório isolado no WSL.
- Comandos: `init <nome>`, `save <nome> -m "<msg>"`, `load <nome>`.
- O comando `load` joga os arquivos .md concatenados direto no `clip.exe` (área de transferência do Windows) para serem colados no prompt da IA.
