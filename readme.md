## Criando página de recuperação
1. Posso copiar a página de SignIn e fazer as alterações necessárias como nome de variável e remoção de alguns inputs.
2. Criar a nova rota para recuperação de senha.

## Enviando formulário a API
1. Ir na página ForgotPassword e conectar a API(/password/forgot) depois que é feita a válidação do Yup.
2. Posso criar uma variável de estado para que seja possível exibir uma mensagem *Carregando...* no botão de envio quando a requisição for feita. E setar *false* no *finally* do try/catch, já que independente de sucesso ou não, a mensagem *Carregando...* deve desaparecer.

> Tomar cuidado para **NÃO** passar valores booleanos **tags html**(diretamente a elas). Irá acusar erro no console do navegador.

> Nesses casos, eu posso converter o valor booleano para número, assim caso seja **true** será convertido para **1** e se for **false** será convertido para **0**.

> Pode desabilitar a verificação ortográfica de uma tag html passando *spellCheck="false"*

## Criação de página de resetar senha
1. Posso seguir o mesmo esquema de criação de página da página de *SignIn* e inserir os inputs de *password* e *password_confirmation* no *Form*.

2. Checar com a validação do Yup se as senhas são iguais utilizando:
```typescript
const schema = Yup.object().shape({
  password: Yup.string().required('Senha obrigatória'),
  password_confirmation: Yup.string()
    .oneOf([Yup.ref('password')], 'Confirmação incorreta')
    .required('Confirmação obrigatória'),
});
```

3. Caso tudo dê certo, redicionar o *user* para a página de *SignIn*.

4. Adicionar a rota reset-password

## Conectando a API para resetar senha
1. Conectar a api na rota */password/reset*, enviando *password*, *password_confirmation* e o *token*.

2. Para receber o *token* do *query params*, utilizar do react-router-dom o hook **useLocation**.
```typescript
import { useHistory, useLocation } from 'react-router-dom';

const { search } = useLocation();

// ou posso utilizar
// const token = search.replace('?token=', '');
const [, token] = search.split('?token=');

if (!token) {
  throw new Error();
}

await api.post('/password/reset', {
  password,
  password_confirmation,
  token,
});
```
## Criando Header
1. Fazer a estruturação da página
```typescript
import React from 'react';

import imgLogo from '../../assets/logo.svg';
import { useAuth } from '../../hooks/auth';

import { Container, Header, HeaderContent, Profile } from './styles';

const Dashboard: React.FC = () => {
  const { signOut, user } = useAuth();

  return (
    <Container>
      <Header>
        <HeaderContent>
          <img src={imgLogo} src="GoBarber" />

          <Profile>
            <img src={user.avatar_url} src={user.name} />
            <div>
              <span>Bem vindo,</span>
              <strong>{user.name}</strong>
            </div>
          </Profile>

          <button type="button" onClick={signOut}>
            <FiPower />
          </button>

        </HeaderContent>
      </Header>

    </Container>
  );
}

export default Dashboard;

```
2. Fazer a estilização
```typescript
import styled from 'styled-components';

export const Container = styled.div`
  
`;

export const Header = styled.header`
  padding: 32px 0;
  background: #28262e;
  
`;

export const HeaderContent = styled.div`
  max-width: 1120px;
  margin: 0 auto;
  display: flex;
  align-items: center;


  > img {
    height: 80px;
  }

  button {
    margin-left: 0;
    background: transparent;
    border: 0;

    svg {
      color: #999591;
      height: 20px;
      width: 20px;
    }
  }
`;

export const Profile = styled.div`
  display: flex;
  align-items: center;
  margin-left: 80px;

  img {
    height: 56px;
    width: 56px;
    border-radius: 50%;
  }

  div {
    display: flex;
    flex-direction: column;
    line-height: 24px;
    margin-left: 16px;

    span {
      color: #f4ede8;
    }

    strong {
      color: #ff9000;
    }
  }
`;

```
3. Criar a interface para *User* no hook auth
```typescript

interface User {
  id: string;
  avatar_url: string;
  name: string;
}

interface AuthState {
  token: string;
  user: User;
}

...

```
