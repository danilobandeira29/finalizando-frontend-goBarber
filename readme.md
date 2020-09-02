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

## Próximo agendamento
1. Fazer a estruturação do Content(Schedule, NextAppointment, Calendar)

```typescript
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

      <Content>
        <Schedule>
          <h1>Horários agendados</h1>
          <p>
            <span>Hoje</span>
            <span>Dia 06</span>
            <span>Segunda-feira</span>
          </p>

          <NextAppointment>
          <strong>Atendimento a seguir</strong>
          <div>
                <img
                  src="https://avatars3.githubusercontent.com/u/61798805?s=460&u=e3c897fa042ae6b54fa8b262b6f146d8174cb9eb&v=4"
                  alt="Danilo Bandeira"
                />
                <strong>Danilo Bandeira</strong>
                <span>
                  <FiClock />
                  08:00
                </span>
              </div>
          </NextAppointment>
        </Schedule>
        <Calendar />
      </Content>

    </Container>
  );
}

export default Dashboard;

```

2. Fazer a estilização
```typescript
import styled from 'styled-components';

export const Content = styled.main`
  max-width: 1120px;
  margin: 64px auto;
  display: flex;
`;

export const Schedule = styled.section`
  flex: 1;
  margin-right: 120px;

  h1 {
    font-size: 36px;
  }

  p {
    margin-top: 12px;
    color: #ff9000;
    display: flex;
    font-weight: 500;
    align-items: center;

    span + span {
      margin-left: 8px;
      display: flex;
      align-items: center;
    }

    span + span::before {
      content: '';
      width: 2px;
      height: 12px;
      background: #ff9000;
      margin-right: 8px;
    }
  }
`;

export const NextAppointment = styled.div`
  margin-top: 64px;

  > strong {
    color: #999591;
    font-size: 20px;
    font-weight: 400px;
  }

  div {
    background: #3e3b47;
    border-radius: 10px;
    padding: 16px 24px;
    margin-top: 24px;

    display: flex;
    align-items: center;

    position: relative;

    &::before {
      position: absolute;
      height: 80%;
      width: 2px;
      left: 0;
      top: 10%;
      content: '';
      background: #ff9000;
      border-radius: 10px 0 0 10px;
      transform: matrix(-1, 0, 0, 1, 0, 0);
    }

    img {
      height: 80px;
      width: 80px;
      border-radius: 50%;
    }

    strong {
      margin-left: 24px;
      font-size: 24px;
      color: #f4ede8;
    }

    span {
      margin-left: auto;
      color: #999591;
      display: flex;
      align-items: center;
      font-size: 20px;

      svg {
        height: 20px;
        width: 20px;
        color: #ff9000;
        margin-right: 12px;
      }
    }
  }
`;

export const Calendar = styled.aside`
  width: 380px;
`;

```
## Listagem de agendamentos
1. Fazer a estruturação da listagem de agendamentos

```typescript
<Section>
  <strong>Manhã</strong>

  <Appointment>
    <span>
      <FiClock />
      08:00
    </span>

    <div>
      <img
        src="https://avatars3.githubusercontent.com/u/61798805?s=460&u=e3c897fa042ae6b54fa8b262b6f146d8174cb9eb&v=4"
        alt="Danilo Bandeira"
      />
      <strong>Danilo Bandeira</strong>
    </div>

  </Appointment>
</Section>

```

2. Fazer a estilização
```typescript
import styled from 'styled-components';

export const Section = styled.section`
  margin-top: 48px;

  > strong {
    color: #999591;
    border-bottom: 1px solid #3e3b47;
    font-size: 20px;
    line-height: 26px;
    display: block;
    padding-bottom: 16px;
    margin-bottom: 16px;
  }
`;

export const Appointment = styled.div`
  display: flex;
  align-items: center;

  & + div {
    margin-top: 16px;
  }

  span {
    display: flex;
    align-items: center;
    color: #f4ede8;

    svg {
      height: 17px;
      width: 17px;
      color: #ff9000;
      margin-right: 8px;
    }
  }

  div {
    background: #3e3b47;
    display: flex;
    align-items: center;
    padding: 16px 24px;
    border-radius: 10px;
    flex: 1;
    margin-left: 26px;

    img {
      height: 56px;
      width: 56px;
      border-radius: 50%;
    }

    strong {
      margin-left: 16px;
      font-size: 20px;
      color: #f4ede8;
    }
  }
`;
```
