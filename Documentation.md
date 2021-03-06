# Iniciando Projeto
- yarn create next-app (nome-app)

# Adicionando Typescript
- yarn add typescript @types/react @types/node -D

# Executando em produção
- yarn build
- yarn start

# Executando server e modo dev
- yarn dev
- yarn server

# Adicionando sass
- yarn add sass

# Adicionando lib date-fns para lidar com datas dentro do JS
- yarn add date-fns

# Criando pasta Header para receber todos os components do Header da aplicação

# Adicionando Header ao arquivo _app.tsx pois header irá aparecer em todas as pages da aplicação

# Configurando css do arquivo _app para deixar o header flexivel para receber o component Player ao lado

# Instalando JsonServer para simular uma api
- yarn add json-server -D
- criando server.json na raiz do projeto
- atualize o package.json (
     
    "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "server": "json-server server.json -w -d 750 -p 3333"
  },
)
- yarn server
- localhost:3333/episodes

# Maneiras de se consumir uma Api:

# Modelo SPA(single page applications) usando useEffect:

 useEffect(() => {
    fetch('http://localhost:3333/episodes').then((response) => {
      return response.json()
    })
      .then((data) => {
        console.log(data)
      })
  }, [])


# Modelo SSR(server side redering), requisição feita na camada do next

- Dentro de qualquer arquivo na pasta pages export uma function chamada (getServerSideProps)
     
    export async function getServerSideProps(){
        const response = await fetch('http://localhost:3333/episodes')
        const data = await response.json()

        return{
            props:{
                episode: data
            }
        }
}


# Modelo SSG(static site generators), requisição feita na camada do next

- Dentro de qualquer arquivo na pasta pages export uma function chamada (getStaticProps)
     
    export async function getStaticProps(){
      //Limitando o retorno da api a 12 registros com ?_limit=12
        const response = await fetch('http://localhost:3333/episodes?_limit=12')
        const data = await response.json()

        return{
            props:{
                episode: data
            },

            //a cada 8 horas ira gerar uma nova versão desta pagina
            revalidate: 60 * 60 *8,

        }
}

# Adicionando axios
- yarn add axios
- criando no src uma pasta services/api.ts(

  import axios from 'axios'

export const api = axios.create({
    baseURL: 'http://localhost:3333/'
})

)

- Atualizando requisição para a adição do typescript SSG (

  
  import { GetStaticProps } from 'next'


  export const getStaticProps: GetStaticProps = async () => {
  const { data } = await api.get('episodes', {
    params: {
      _limit: 12,
      _sort: 'published_at',
      _order: 'desc'
    }
  });

  return {
    props: {
      episodes: data
    },

    revalidate: 60 * 60 * 8,
  }

)

# Formatando os dados logo depois de feita a requisição no servidor para melhorar a performance