# CapRover Deployment Options

Este projeto oferece 3 opções de deploy para CapRover:

## Opção 1: Imagen Pré-construída (RECOMENDADO) 🚀

**Arquivo**: `Dockerfile.simple`
**Tempo**: ~2-5 minutos
**Tamanho**: ~8GB

```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile.simple"
}
```

**Vantagens**:
- Deploy mais rápido
- Menos chance de erro
- Usa imagem oficial testada
- Menos recursos necessários durante build

## Opção 2: Build Completo Compatível

**Arquivo**: `Dockerfile.caprover`
**Tempo**: ~30-60 minutos
**Tamanho**: ~8GB

```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile.caprover"
}
```

**Vantagens**:
- Build customizável
- Compatível com CapRover (sem BuildKit)
- Controle total sobre dependências

## Opção 3: Build Otimizado (Requer BuildKit)

**Arquivo**: `Dockerfile`
**Tempo**: ~20-40 minutos
**Tamanho**: ~8GB

⚠️ **Não funciona no CapRover padrão** (requer BuildKit)

## Como Trocar a Opção

Edite o arquivo `captain-definition`:

```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile.simple"
}
```

Substitua `Dockerfile.simple` por:
- `Dockerfile.simple` → Imagem pré-construída (RÁPIDO)
- `Dockerfile.caprover` → Build completo compatível
- `Dockerfile` → Build otimizado (só se CapRover tiver BuildKit)

## Configuração de Recursos

Para qualquer opção, configure no CapRover:

### Recursos Mínimos:
- **RAM**: 4GB
- **CPU**: 2 cores  
- **Disk**: 15GB

### Recursos Recomendados:
- **RAM**: 8GB+
- **CPU**: 4+ cores
- **Disk**: 25GB+

### Variáveis de Ambiente:
```env
DOCLING_SERVE_ENABLE_UI=1
UVICORN_HOST=0.0.0.0
UVICORN_PORT=80
UVICORN_WORKERS=1
OMP_NUM_THREADS=2
```

## Troubleshooting

### Build Timeout
Se o build der timeout, aumente o timeout no CapRover ou use `Dockerfile.simple`.

### Out of Memory  
- Reduza `UVICORN_WORKERS` para 1
- Aumente RAM do servidor
- Use `Dockerfile.simple`

### Startup Lento
Primeira inicialização pode demorar 5-10 minutos baixando modelos de IA.

## Monitoramento

Após deploy, monitore:
- **Logs**: CapRover dashboard
- **Health**: `https://seuapp.com/docs`
- **UI**: `https://seuapp.com/ui`