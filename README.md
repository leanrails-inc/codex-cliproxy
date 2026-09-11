# Codex via CartPanda CLIProxyAPI

Configuração para usar o Codex (app desktop e CLI) através do CLIProxyAPI da CartPanda.

É um único arquivo. Não precisa de extensão nem de API key.

## Requisitos

- Codex instalado (app desktop ou CLI).
- Tailscale conectado. O proxy só é acessível pela Tailnet.
- Login no Codex com sua conta ChatGPT, como de costume.

## Onde colocar o arquivo

| Sistema | Destino                                  |
|---------|------------------------------------------|
| macOS   | `~/.codex/config.toml`                   |
| Windows | `C:\Users\<voce>\.codex\config.toml`     |

A pasta `.codex` é criada na primeira vez que você roda o Codex. Se ainda não existir, crie.

### macOS (Terminal)

```bash
git clone https://github.com/leanrails-inc/codex-cliproxy.git ~/Downloads/codex-cliproxy
mkdir -p ~/.codex
cp ~/Downloads/codex-cliproxy/config.toml ~/.codex/config.toml
```

### Windows (PowerShell)

```powershell
git clone https://github.com/leanrails-inc/codex-cliproxy.git "$env:USERPROFILE\Downloads\codex-cliproxy"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex" | Out-Null
Copy-Item "$env:USERPROFILE\Downloads\codex-cliproxy\config.toml" "$env:USERPROFILE\.codex\config.toml"
```

Sem git? Baixe o `config.toml` direto pelo GitHub (botão Download) e copie para o destino acima.

## Se você já tem um config.toml

Não sobrescreva. Abra o `~/.codex/config.toml` e:

1. Adicione `model_provider = "cliproxyapi"` no topo, logo abaixo da linha `model = ...`.
2. Cole o bloco inteiro `[model_providers.cliproxyapi]` do `config.toml` deste repo no final do arquivo.

## Depois de copiar

1. Abra o Codex (app ou terminal) e faça login com a conta ChatGPT se ele pedir.
2. No app desktop, abra uma thread NOVA. Threads abertas antes da mudança continuam no provider antigo.
3. Escolha o modelo no seletor normal (GPT-6 Astra, GPT-5.6 Sol, etc). Não existe seletor separado do cliproxy.

## Teste

```bash
codex exec "Responda exatamente: CLIPROXY OK"
```

Saída esperada: `CLIPROXY OK`.

Para confirmar que o app desktop está usando o proxy, abra o arquivo mais recente em `~/.codex/sessions/` e verifique se a primeira linha contém `"model_provider":"cliproxyapi"`.

## Observações

- O Codex pode avisar que não conseguiu carregar o catálogo de plugins do ChatGPT. É inofensivo.
- Para trocar o modelo padrão, edite a linha `model =`. Modelos GPT disponíveis: gpt-6-astra, gpt-5.6-sol, gpt-5.6-terra, gpt-5.6-luna, gpt-5.5, gpt-5.3-codex-spark.
- Modelos Claude, Gemini e Grok do proxy não aparecem no Codex. Para eles use o pi com a extensão `cartpanda-cliproxy`.
