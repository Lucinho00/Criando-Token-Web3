# Criando-Token-Web3

Criar um token nos padrões Web3 envolve a criação de um contrato inteligente na blockchain, geralmente seguindo o padrão ERC-20 para tokens na rede Ethereum. Aqui está um guia passo a passo para criar um token do zero.

1. Preparação do Ambiente
Antes de começar, você precisará configurar seu ambiente de desenvolvimento:

Instalar o Node.js: Se ainda não tiver o Node.js instalado, baixe-o aqui.
Instalar o Truffle ou Hardhat: Essas ferramentas facilitam o desenvolvimento, teste e deploy de contratos inteligentes.
Para Truffle: npm install -g truffle
Para Hardhat: npm install --save-dev hardhat
Instalar MetaMask: Uma carteira Ethereum para interagir com a blockchain.

Criar um Projeto Truffle ou Hardhat
Vamos usar o Hardhat como exemplo.

Inicialize um novo projeto:
npx hardhat

Escolha "Create a basic sample project" e siga as instruções.

Instalar Dependências:

npm install @openzeppelin/contracts


Escrever o Contrato Inteligente
Crie um arquivo chamado MyToken.sol dentro da pasta contracts no seu projeto.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}


Aqui está o que cada parte faz:

ERC20: É o padrão para tokens fungíveis.
MyToken: É o nome do seu token.
MTK: É o símbolo do seu token.
initialSupply: É o suprimento inicial de tokens que serão criados.
4. Compilar o Contrato
Para compilar o contrato inteligente, execute o seguinte comando:

npx hardhat compile


Deploy do Contrato
Crie um script de deploy em scripts/deploy.js:

async function main() {
    const [deployer] = await ethers.getSigners();
    console.log("Deploying contracts with the account:", deployer.address);

    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy(1000000 * 10 ** 18); // 1 milhão de tokens

    console.log("Token deployed to:", token.address);
}

main()
   .then(() => process.exit(0))
   .catch((error) => {
       console.error(error);
       process.exit(1);
   });

Este script faz o deploy do contrato e cria 1 milhão de tokens.

6. Deploy na Rede Teste
Antes de fazer o deploy na mainnet, você pode testar na rede de testes Rinkeby ou Goerli.

Configure o Hardhat para a rede de testes:

Em hardhat.config.js adicione:

require("@nomiclabs/hardhat-waffle");

module.exports = {
  solidity: "0.8.0",
  networks: {
    rinkeby: {
      url: `https://rinkeby.infura.io/v3/YOUR_INFURA_PROJECT_ID`,
      accounts: [`0x${YOUR_PRIVATE_KEY}`],
    },
  },
};


Deploy:

npx hardhat run scripts/deploy.js --network rinkeby


Após o deploy, você verá o endereço do contrato na rede de testes.

7. Interagir com o Token
Você pode usar o Etherscan para verificar e interagir com o contrato na rede de testes, ou usar ferramentas como MyEtherWallet ou MetaMask.

8. Deploy na Mainnet
Quando estiver pronto para lançar o token na mainnet, altere o script de configuração do Hardhat para a rede principal Ethereum, Polygon, ou outra blockchain, e siga os mesmos passos.

Resumo do Token
Nome: MyToken
Símbolo: MTK
Suprimento Inicial: 1 milhão de tokens
Esse é um exemplo básico, mas você pode adicionar funcionalidades extras, como queimadura de tokens (burn), pausabilidade, ou governança. Para uma personalização mais profunda ou outros padrões, o OpenZeppelin oferece contratos auditados e modulares que podem ser utilizados.

