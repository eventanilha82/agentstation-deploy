# Agent Station — Implantação OCI

Implante uma Station privada de agentes na sua própria tenancy Oracle Cloud
Infrastructure (OCI). Este repositório contém somente a documentação de
implantação e os assets imutáveis consumidos pelo OCI Resource Manager.

> O botão **Implantar no Oracle Cloud** será ativado quando o pacote público
> imutável da release `v0.1.0` for publicado e verificado. O repositório não
> oferece um link de implantação ativo antes dessa publicação.

## O que será criado

Quando ativado, o botão abrirá o OCI Resource Manager com o pacote `v0.1.0` já
selecionado. A pessoa que implanta escolhe o compartment e a região, revisa o
plano e confirma o apply. Nenhum recurso será criado pelo simples clique no
botão.

O pacote instala uma Station com:

- runtime local CPU-only `llama.cpp` e o perfil AgentWorld homologado;
- Open WebUI, OpenClaw, Hermes e Agent Zero;
- memória/contexto OpenViking, ferramentas MCP e automação Activepieces;
- HTTPS, página inicial, ativação cifrada por `age` e recuperação operacional;
- limites de rede, credenciais geradas dentro da VM e armazenamento privado.

## Antes de implantar

1. Tenha uma conta OCI com permissão para criar os recursos no compartment
   escolhido.
2. Gere uma identidade local `age` e informe somente sua chave pública
   `age1...` no formulário. A chave privada nunca deve ser enviada à OCI.
3. Revise o plano e os custos antes de aplicar. A implantação cria recursos na
   sua conta e a cobrança é responsabilidade da sua tenancy.

Depois do apply, use os outputs de ativação para baixar o bundle cifrado,
decifrá-lo localmente e concluir a confirmação. A Station gera suas credenciais
internas na VM; elas não entram no estado Terraform nem neste repositório.

## Assets verificáveis

A release pública disponibilizará exatamente dois arquivos:

| Asset | Uso |
| --- | --- |
| `agent-station-oci-stack.zip` | Pacote Terraform/bootstrapping consumido pelo OCI Resource Manager. |
| `agent-station-oci-stack.zip.sha256` | Checksum para validar o ZIP antes do uso. |

Valide o download em um diretório limpo:

```bash
shasum -a 256 -c agent-station-oci-stack.zip.sha256
```

No Linux, use `sha256sum -c agent-station-oci-stack.zip.sha256`.

## Limite entre público e privado

O código completo, evidências operacionais, CI e imagem homologada permanecem
privados. O ZIP público contém somente os arquivos necessários para Terraform,
`cloud-init` e bootstrap instalarem a Station. Ele não inclui credenciais,
estado, dados de usuários, benchmarks, evidências ou o serviço privado de
Registry.

## Suporte e operação

Esta é a distribuição de implantação. A operação posterior acontece dentro da
Home da sua Station: ativação, status dos componentes e links permitidos são
mostrados sem expor tokens. Integrações de aplicativos remotos, como Conduit,
devem usar a tela de onboarding própria do produto quando estiverem disponíveis;
não copie credenciais da interface de status.

Para documentação geral, segurança e procedimentos de release, consulte o
projeto Agent Station privado da sua organização.
