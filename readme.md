# 📝 Sobre
Anotações que faço ao longo dos estudos sobre:
- ReactJS
- Hooks (context api, useCallback, useMemo,...)
- API REST
- TDD(Test-Driven Development)

# 🏆 Desafio
- Anotar a forma que resolvo os problemas, traçando caminhos e afins.
- Colocar em prática os conhecimentos que são adquiridos diáriamente nos meus estudos.

# 👀 Projetos nos quais estou aplicando esses conceitos
Disponível em: [Frontend GoBarber](https://github.com/danilobandeira29/frontend-gobarber-web)

---

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

## Calendário e estilizações
1. Fazer uso da lib react-day-picker
```typescript
import React, { useState, useCallback } from 'react';
import DayPicker, { DayModifiers } from 'react-day-picker';
import 'react-day-picker/lib/style.css';

const Dashboard: React.FC = () => {

  const [selectedDate, setSelectedDate] = useState(new Date());

  const handleDateChange = useCallback((day: Date, modifiers: DayModifiers ) => {
    if(modifiers.available){
      setSelectedDate(day);
    }
  }, []);

  return (
    ...

    <Calendar>
      <DayPicker
        weekdaysShort={['D', 'S', 'T', 'Q', 'Q', 'S', 'S']}
        fromMonth={new Date()}
        disabledDays={{ daysOfWeek: [0, 6] }}
        modifiers={{
          available: { daysOfWeek: [1, 2, 3, 4, 5, 6] },
        }}
        selectedDays={selectedDate}
        onDayClick={handleDateChange}
        months={[
          'Janeiro',
          'Fevereiro',
          'Março',
          'Abril',
          'Maio',
          'Junho',
          'Julho',
          'Agosto',
          'Setembro',
          'Outubro',
          'Novembro',
          'Dezembro',
        ]}
      />
    </Calendar>
  );
}
```

2. Fazer a estilização do Calendar
```typescript
import styled from 'styled-components';
import { shade } from 'polished';

export const Calendar = styled.div`
  .DayPicker {
    border-radius: 10px;
  }

  .DayPicker-wrapper {
    padding-bottom: 0;
    background: #3e3b47;
    border-radius: 10px;
  }

  .DayPicker,
  .DayPicker-Month {
    width: 100%;
  }

  .DayPicker-NavButton {
    color: #999591 !important;
  }

  .DayPicker-NavButton--prev {
    right: auto;
    left: 1.5em;
    margin-right: 0;
  }

  .DayPicker-Month {
    border-collapse: separate;
    border-spacing: 8px;
    margin: 16px 0 0 0;
    padding: 16px;
    background-color: #28262e;
    border-radius: 0 0 10px 10px;
  }

  .DayPicker-Caption {
    margin-bottom: 1em;
    padding: 0 1em;
    color: #f4ede8;

    > div {
      text-align: center;
    }
  }

  .DayPicker-Day {
    width: 40px;
    height: 40px;
  }

  .DayPicker-Day--available:not(.DayPicker-Day--outside) {
    background: #3e3b47;
    border-radius: 10px;
    color: #fff;
  }

  .DayPicker:not(.DayPicker--interactionDisabled)
    .DayPicker-Day:not(.DayPicker-Day--disabled):not(.DayPicker-Day--selected):not(.DayPicker-Day--outside):hover {
    background: ${shade(0.2, '#3e3b47')};
  }

  .DayPicker-Day--today {
    font-weight: normal;
  }

  .DayPicker-Day--disabled {
    color: #666360 !important;
    background: transparent !important;
  }

  .DayPicker-Day--selected {
    background: #ff9000 !important;
    border-radius: 10px;
    color: #232129 !important;
  }
`;

```

## Disponibilidade do mês

1. Preciso armazenar o mês atual que o usuário selecionou no *day picker*, pois eu preciso fazer uma requisição na api TODA vez que o usuário alterar o mês, ou seja, **preciso de um efeito coleteral(useEffect)**.
2. Criar uma função para lidar com *onMonthChange* do *day picker*.
```typescript
interface MonthAvailabilityItem {
  day: number;
  available: boolean;
}

const [currentMonth, setCurrentMonth] = useState(new Date());
const [monthAvailability, setMonthAvailability] = useState<
  MonthAvailabilityItem[]
>([]);


const handleMonthChange = useCallback((month: Date) => {
  setCurrentMonth(month);
}, []);

useEffect(() => {
  api.get(`/providers/${user.id}/month-availability`, {
    params: {
      year: currentMonth.getFullyear(),
      month: currentMonth.getMonth() + 1,
    }
  }).then(response => setMonthAvailability(response.data));
}, [currentMonth]);

```
### Nunca devo executar funções de alterações/conversão de valores direto no componente(no return, em tempo de renderização)!! Pois qualquer cálculo dentro do return do componente, ele irá renderizar TODO o componente novamente.
### Os hooks foram criados pensando nisso, em renderizações desnecessárias.

Um bom exemplo disso é o *useCallback*, que só irá renderizar a função uma única vez(de acordo com o array de dependências).

Exemplo: 
```typescript
const handleSubmit = useCallback((data: DataForm) => {
  ...
}, []);

```
Nesse caso, caso o componente seja renderizado mais de uma vez, essa função não sofrerá essa renderização.

Existem também o **useMemo**, que irá fazer alguma alteração/conversão/cálculo somente quando necessário.

```typescript
const disabledDays = useMemo(() => {
  const dates = monthAvailability
  .filter(monthDay => monthDay.available === false)
  .map(monthDay => {
    const year = currentMonth.getFullYear();
    const month = currentMonth.getMonth();

    const date = new Date(year, month, monthDay.day);
  });

  return dates;
}, [currentMonth, monthAvailability]);

```
3. Passar essa variável *disabledDays* para dentro do *disabledDays* do **daypicker**.

4. Enviar o token para que seja possível fazer a requisição. Ir na context api *auth.ts* e setar o token no headers do axios, **tanto no signIn quando o usuário der F5 na página(localStorage)**.

```typescript
api.defaults.headers.authorization = `Bearer ${token}`

```

## Agendamentos da API
1. Devo alterar a informação que se refere a data da listagem de agendamentos
```typescript
import React, { useMemo, useState } from 'react';
import { isToday, format } from 'date-fns';
import ptBR from 'date-fns/locale/pt-BR';



const selectedDayAsText = useMemo(() => {
  return format(selectedDate, "'Dia' dd 'de' MMMM", 
    {
      locale: ptBR,
    }
  );
}, [selectedDate]);

const selectedWeekAsText = useMemo(() => {
    return format(selectedDate, 'cccc', {
      locale: ptBR,
    });
  }, [selectedDate]);
});

return (
  ...
  <Schedule>
    <h1>Horários agendados</h1>
    <p>
      {isToday(selectedDate) && <span>Hoje</span>}
      <span>{selectedDateAsText}</span>
      <span>{selectedWeekAsText}</span>
    </p>
  ...
  </Schedule>
);
```

### Todo Hook funciona dessa forma: vou disparar algo, sempre que isso mudar. O que muda é apenas o retorno.

- useEffect executa uma função
- useCallback retorna uma função
- useMemo retorna um valor(valor já calculado)

2. Conectar a API para fazer a listagem de agendamentos e verificar se está retornando.
```typescript
// não preciso tipar todo que vem de retorno da api
interface Appointments {
  id: string;
  date: string;
  user: {
    name: string;
    avatar_url: string;
  };
};

const [selectedDate, setSelectedDate] = useState(new Date());
const [appointments, setAppointments] = useState<Appointments[]>([])

useEffect(() => {
  api.get('/appointments/me', {
    params: {
      year: selectedDate.getFullYear(),
      month: selectedDate.getMonth() + 1,
      day: selectedDate.getDate(), 
    }
  }).then(response => setAppointments(response.data));

  console.log(appointments);
}, [selectedDate]);

```

## Exibindo agendamentos em tela
1. Primeiro, deve ser feito a separação dos appointments pelo turno(manhã, tarde). Para isso, será feito um cálculo(useMemo) para que o valor retornado seja da listagem de appointments antes das 12h e depois ou igual as 12h.
```typescript
const morningAppointments = useMemo(() => {
  return appointments.map(appointment => parseISO(appointment.date).getHours() < 12 );
}, [appointments]);

const afternoonAppointments = useMemo(() => {
  return appointments.map(appointment => parseISO(appointment.date).getHours() >= 12 );
}, [appointments]);

```
2. Fazer a listagem de appointments de acordo com o turno
```typescript
const Dashboard: React.FC = () => {
  ...
const morningAppointments = useMemo(() => {
  return appointments.map(appointment => parseISO(appointment.date).getHours() < 12 );
}, [appointments]);

const afternoonAppointments = useMemo(() => {
  return appointments.map(appointment => parseISO(appointment.date).getHours() >= 12 );
}, [appointments]);

  return (

    ...
    {morningAppointments.map(appointment => {
      <Appointment key={appointment.id}>

        <span>
          <FiClock />
          08:00
        </span>

        <div>
          <img
            src={appointment.user.avatar_url}
            alt={appointment.user.name}
          />
          <strong>{appointment.user.name}</strong>
        </div>

      </Appointment>
    })}
  );
}
```

3. Fazer a formatação da hora quando tiver os dados da API, ou seja, no useEffect. Logo em seguida, fazer a alteração no appointment para exibir a hora do agendamento dinamicamente({appointment.hourFormatted}).
```typescript
interface Appointments {
  id: string;
  date: string;
  hourFormatted: string;
  user: {
    name: string;
    avatar_url: string;
  };
}

useEffect(() => {
  api
    .get<Appointments[]>('/appointments/me', {
      params: {
        year: selectedDate.getFullYear(),
        month: selectedDate.getMonth() + 1,
        day: selectedDate.getDate(),
      },
    })
    .then(response => {
      const appointmentFormatted = response.data.map(appointment => {
        return {
          ...appointment,
          hourFormatted: format(parseISO(appointment.date), 'HH:mm'),
        };
      });

      setAppointments(appointmentFormatted);
    });
}, [selectedDate]);

```
### Não fazer formatação de dados dentro do return do React!!

## Finalizando listagem de agendamentos
1. Mostrar o *Agendamento a seguir* de forma correta. Criar um cálculo para pegar o primeiro item do array de agendamentos(já que eles já estão em ordem) e verificar se ele é depois do horário atual.

```typescript
const nextAppointment = useMemo(() => {
  return appointments.find(appointment => isAfter(parsedISO(appointment.date), new Date()))
}, [appointments]);


 {isToday(selectedDate) && nextAppointment && (
    <NextAppointment>
      <strong>Agendamento a seguir</strong>

      <div>
        <img
          src={nextAppointment.user.avatar_url}
          alt={nextAppointment.user.name}
        />
        <strong>{nextAppointment.user.name}</strong>
        <span>
          <FiClock />
          {nextAppointment.hourFormatted}
        </span>
      </div>
    </NextAppointment>
  )}

```
## Página de perfil
1. Fazer a criação da page de Perfil e tentar estilizar de acordo com o figma.
> Posso utilizar margin negativa pra estilizar o formulário de acordo.

## Troca de avatar
1. Inserir no button do *AvatarProfile* um *input* do tipo *file*.
2. Esse button agora será um label, com a prop htmlFor="id_do_input_file".
3. Adicionar a prop *onChange* no input element, para que ele chame a função handleAvatarChange, que irá receber e: ChangeEvent<HTMLInputElement>

```typescript
const handleAvatarChange = useCallback((event: ChangeEvent<HTMLInputElement>) => {
  if (event.target.files){
    const data = new FormData();

    data.append('avatar', event.target.files[0]);

    api.patch('/users/avatar', data).then(response => {
      updateUser(response.data);

      addToast({
        type: "success",
        title: "Avatar atualizado!",
      })
    });
  }
}, [updateUser]);

return (
  ...
  <AvatarProfile>
    <img src={user.avatar_url} alt={user.name} />
    <label htmlFor="avatar" >
      <input 
        type="file"
        id="avatar"
        onChange={handleAvatarChange}
        style={{ display: none }}
      />
    </label>
  </AvatarProfile>
);
```

4. Ir no hook *auth.ts* e fazer a criação do novo método updateUser
```typescript
  ...
  const updateUser = useCallback((user: User) => {
    setData({
      token: data.token,
      user
    });
    localStorage.setItem('GoBarber:user', JSON.stringify(user));
  }, [data.token]);
```

## Alteração de dados
1. Fazer validação dos inputs utilizando o Yup.
2. Fazer a chamada a api para a rota profile pra que seja possível atualizar: nome, email, senha.

```typescript
const handleSubmitForm = (async(data: ProfileFormData) => {
  const schema = Yup.object().shape({
    name: Yup.string().required('Nome obrigatório'),
    email: Yup.string().required('E-mail obrigatório').email('Dgitei um email válido'),
    old_password: Yup.string(),
    password: Yup.string().when('old_password', {
      is: value => !!value.length,
      then: Yup.string().required('Nova senha obrigatória'),
      otherwise: Yup.string(),
    }),
    password_confirmation: Yup.string().when('old_password', {
      is: value => !!value.length,
      then: Yup.string().required('Confirmação obrigatória'),
      otherwise: Yup.string(),
    }).oneOf([Yup.ref('password')], 'Confirmação incorreta'),
  }),

  await schema.validate(data, {
    abortEarly: false,
  });

  const { name, email, old_password, password, password_confirmation } = data;

  // dessa forma será possível atualizar apenas o name e email sem passar a senha
  const formData = Object.assign({
    name, email,
  }, old_password ? { old_password, password, password_confirmation } : {}),

  // ou posso utilizar o spread operator
  // const formData = {
  //   name, email, 
  // ...(old_password 
  //   ? { old_password, password, password_confirmation } 
  //   : {}),
  // }

  const response = await api.put('/profile', formData);

  updateUser(response.data);

  addToast({
    type: 'success',
    title: 'Perfil atualizado',
  });

  history.push('/dashboard');

})
```
