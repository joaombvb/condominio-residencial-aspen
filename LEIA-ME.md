# Condomínio Edílico Aspen — site institucional

Site de página única, sem build e sem dependências. Abra o `index.html` no
navegador e ele já funciona.

```
index.html              o site inteiro (HTML + CSS + JS)
manifest.webmanifest    instalação como app (PWA)
sw.js                   cache offline
robots.txt
sitemap.xml
assets/                 onde entram as imagens oficiais
```

---

## 1. Adicionar as imagens oficiais

Coloque os arquivos na pasta `assets/`. O site detecta cada um sozinho e
troca o placeholder — nenhuma linha de código precisa ser alterada.

| Arquivo | Uso | Tamanho sugerido |
|---|---|---|
| `fachada.jpg` | hero e galeria | 1600 × 2133 px (3:4), até ~250 KB |
| `logo-aspen.png` | logotipo (PNG transparente) | 1400 px de largura |
| `og-aspen.jpg` | prévia no WhatsApp e redes | 1200 × 630 px exatos |
| `icon-192.png` · `icon-512.png` · `icon-512-maskable.png` | ícones do app | conforme o nome |
| `icon-180.png` | tela de início do iPhone | 180 × 180 px |

**Enquanto os arquivos não existem:**

- O hero mostra um degradê tirado das cores reais do prédio (céu, pastilha
  verde, tijolo, pedra) e uma etiqueta tracejada avisando o que falta.
- O logotipo é desenhado em vetor (SVG) com as mesmas proporções, o mesmo
  dourado, o mesmo arco e o mesmo raminho de folhas. Ele some assim que o
  `logo-aspen.png` aparecer na pasta.

O hero recorta a foto em `center 42%`. Se o ponto de interesse da fotografia
ficar mais alto ou mais baixo, ajuste esse valor em `.hero__media`.

---

## 2. Publicar

Arraste a pasta inteira para o [Netlify Drop](https://app.netlify.com/drop),
Vercel, Cloudflare Pages ou GitHub Pages. Também funciona em hospedagem
comum via FTP.

Localmente, para testar o PWA e a área de transferência (que exigem HTTPS
ou `localhost`):

```bash
npx serve .
```

---

## 3. URL oficial

O endereço oficial é `https://condominio-residencial-aspen.vercel.app/` e está configurado
em quatro lugares:

- `index.html` — nas tags `canonical`, Open Graph, Twitter e no bloco
  `application/ld+json`
- `robots.txt`
- `sitemap.xml`

---

## 4. Alterar dados de contato

Todos os dados oficiais aparecem em dois lugares: no HTML visível e no
objeto `DATA` no início do `<script>` (usado pelo botão *Salvar contato* e
pelo compartilhamento). Altere os dois.

---

## 5. Estrutura do código

O `index.html` está dividido por comentários que correspondem aos
componentes pedidos, para facilitar uma futura porta para Next.js:

`Header · Hero · Introduction · BentoGrid (IdentificationCard,
ContactCard, AddressCard, AdministrationCard) · Gallery ·
UsefulInformation · Footer · MobileNavigation`

O CSS está em 17 blocos numerados no topo do arquivo, começando pelos
tokens (`:root`) — cores, tipografia, raios, sombras e curvas de animação.
Mudar a paleta inteira é mudar sete linhas.

---

## 6. Detalhes de implementação

- **Acessibilidade** — contraste AA verificado em todos os textos, alvos de
  toque de 44 px ou mais, navegação por teclado com foco visível, HTML
  semântico, `aria-label` em todos os ícones, e `prefers-reduced-motion`
  respeitado.
- **Sem JavaScript** — o conteúdo continua visível e legível.
- **Área de transferência** — usa a API moderna com fallback para
  `execCommand`; em caso de falha, avisa o usuário em vez de falhar em
  silêncio.
- **Compartilhar** — Web Share API quando disponível, cópia do link quando
  não.
- **Salvar contato** — gera um vCard 3.0 marcado como empresa
  (`X-ABShowAs:COMPANY`), então o iPhone exibe "Condomínio Edílico Aspen"
  como organização, não como pessoa.
- **CPF** — não aparece em lugar nenhum da página pública, conforme
  solicitado.
