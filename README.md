 # Documentação - Como Publicar um objeto 3D online com  GitHub Pages

## Introdução

Nesta documentação, vou explicar passo a passo como criar uma página no **GitHub Pages** que mostra um modelo 3D. Essa página foi construída com **Three.js** (uma biblioteca JavaScript para visualizações 3D) e o processo vai do zero até a publicação.

**Objetivo**: Criar uma página simples em HTML e JavaScript que exibe um modelo 3D no navegador, com o objetivo de aprender e aplicar conceitos de desenvolvimento web básico.

> **Preparação**: Crie um **novo repositório** público e sem README por enquanto. Clone o repositório para o seu computador.

## Passo 1: Preparar os Arquivos Locais

Crie os seguintes arquivos na pasta do seu repositório:
```
* index.html – O arquivo principal da página.
* main.js – O arquivo JavaScript que vai carregar o modelo 3D.
* libs/ – Pasta para armazenar as bibliotecas externas (Three.js, GLTFLoader, OrbitControls).
```

**Estrutura de Arquivos**:
```
"NOME DO REPOSIRÓRIO"
├── index.html
├── main.js
├── libs/
│   └── three/
│       ├── three.min.js
│       ├── GLTFLoader.js
│       ├── OrbitControls.js
│       ├── OBJLoader.js
│       └── MTLLoader.js
└── modelos/
    └── "MODELO".glb
```
_Comentário: Os arquivos serão adicionados posteriormente._

## Passo 3: Baixar as Bibliotecas Necessárias

Para carregar o modelo 3D com o Three.js, são necessárias algumas bibliotecas, para isso abrimos cada um dos seguintes link de script no navegador e salve-os individualmente:

> [three.min.js](https://cdn.jsdelivr.net/npm/three@0.150.1/build/three.min.js) - arquivo principal do Three.js

> [GLTFLoader.js](https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/loaders/GLTFLoader.js) – serve pra carregar os modelos 3D no formato `.glb`

> [OrbitControls.js](https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/controls/OrbitControls.js) – adiciona controles como girar, dar zoom, e outros.

> [`OBJLoader.js`](https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/loaders/OBJLoader.js) — para carregar modelos `.obj` _(não usada nesse momento)_

> [`MTLLoader.js`](https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/loaders/MTLLoader.js) — para carregar materiais `.mtl` associados aos `.obj` _(não usada nesse momento)_


* Em cada link, clique com o botão direito na página e selecione a opção "Salvar página como" e salve o arquivo com o mesmo nome original sugerido dentro do diretório `libs/three/`.

* Esses arquivos serão referenciados a seguir no arquivo HTML.

_Comentário: Pra fazer o modelo 3D ser exibido de maneira mais leve e confiável, copiamos os arquivos somente de algumas bibliotecas do Three.js que usamos, em vez de ir no site oficial e baixar um zip completo._

## Passo 4: Criar o Código HTML

No arquivo index.html, copie e cole o seguinte código para estruturar a página:
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Visualizador 3D</title>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>

  <script src="libs/three/three.min.js"></script>
  <script src="libs/three/GLTFLoader.js"></script>
  <script src="libs/three/OrbitControls.js"></script>
</head>
<body>
  <script src="main.js"></script>
</body>
</html>
```

## Passo 5: Criar o Código JavaScript (main.js)

O arquivo `main.js` é o responsável por carregar o modelo 3D no seu visualizador. Nesse arquivo copie e cole o seguinte código:

```
let scene, camera, renderer, controls;

init();
animate();

function init() {
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x222222);

  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.set(0, 1, 3);

  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  controls = new THREE.OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;

  const light = new THREE.HemisphereLight(0xffffff, 0x444444);
  light.position.set(0, 20, 0);
  scene.add(light);

  const loader = new THREE.GLTFLoader();
  loader.load(
    'modelos/dunot.glb',
    function (gltf) {
      scene.add(gltf.scene);
    },
    undefined,
    function (error) {
      console.error('Deu ruim no load do modelo:', error);
    }
  );

  window.addEventListener('resize', onWindowResize, false);
}

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
}

function animate() {
  requestAnimationFrame(animate);
  controls.update();
  renderer.render(scene, camera);
}
```

## Passo 6: Publicar no GitHub Pages

Após atualizar seu repositório com os novos arquivos e diretórios, siga os seguintes passos:

1. Vá até o repositório no GitHub.
2. Clique em "Settings".
3. No menu lateral, encontre a seção GitHub Pages.
4. Em "Source", selecione a branch "main" _(ou outra que estiver usando)_ e clique em "Save".

O GitHub irá gerar um link com a URL onde o novo site está hospedado.
```
PRINCIPAIS DIFICULDADE E DESAFIOS

> Confusão inicial: Começamos sem saber onde colocar os arquivos, como organizar, e nem mesmo qual HTML usar. Ficamos em dúvida entre alguns modelos iniciais e optamos pelo mais simples, mesmo que não seja o mais eficiente.

> Estrutura dos arquivos: Enfrentamos alguns erros devido a estrutura de diretórios. Demoramos para chegar à configuração final conforme foi apresentada.

> Erro CORS: Durante a fase de testes não foi possível rodar a página abrindo direto o HTML no navegador (com "file:///"), devido o erro CORS (Cross-Origin Request Blocked). Tivemos sucesso ao rodar a página em servidor local com GoLive do VSCode antes de colocar no GitHub Pages.

> Caminho errado pro modelo: Em um momento o modelo 3D não aparecia e o erro apontava para o caminho do arquivo ".glb". O "main.js" estava tentando carregar a partir de "modelos/teu_modelo.glb", devido o nome padrão utilizado para construçao do código.

> Iniciante na linguagem: Além dos desafios tecnicos, lidamos com o fato de sermos iniciantes na linguagem e na arquitetura do sistema. A maior parte do dessenvolvimento foi feito com tentativa e erro, copiando, ajustando, entendendo e testando código.