# desafio-aws-iam-run-as-cloud
Aprenda os fundamentos do AWS Identity and Access Management (IAM) por meio de um desafio prático  crie usuários, organize em grupos, aplique permissões com políticas e teste acessos na console AWS. Atividade guiada do Grupo de Estudos Run as Cloud.

> **⚠️ Atenção antes de começar**
>
> Este desafio utiliza recursos reais da AWS. Embora muitos estejam cobertos pelo nível gratuito, **ações como execução de instâncias EC2 podem gerar cobranças**. **Revise sempre o painel de faturamento da AWS e o Cost Explorer após realizar o laboratório.**  
>
> **🛡️ Segurança dos seus dados:** Ao registrar evidências ou publicar resultados no GitHub ou no LinkedIn, **não exponha informações sensíveis**, como:
> - IDs de conta da AWS
> - URLs de login IAM
> - Senhas ou nomes reais
> - Prints com informações pessoais ou sensíveis

## 📚 Objetivo

- Explorar usuários e grupos no IAM.
- Analisar políticas associadas.
- Adicionar usuários aos grupos apropriados.
- Testar os acessos e permissões conforme as políticas.
- Registrar evidências da atividade.

---

## 🏢 Cenário de Negócio

A empresa AnyCloud Solutions está adotando AWS em larga escala.  
Você deve garantir que os membros da equipe tenham **acesso adequado baseado em suas funções**:

| Usuário  | Grupo         | Permissões                                                             |
|----------|----------------|------------------------------------------------------------------------|
| user-1   | S3-Support     | Leitura (ReadOnly) ao Amazon S3                                        |
| user-2   | EC2-Support    | Leitura (ReadOnly) ao Amazon EC2                                       |
| user-3   | EC2-Admin      | Visualizar, iniciar e parar instâncias EC2 (permissões personalizadas) |

---

## 🚀 ETAPA 1 – Criar Usuários IAM

### 📍 Acesse o IAM

1. Acesse o **Console de Gerenciamento da AWS**.
2. No campo de busca, digite `IAM` e selecione o serviço.

### 👤 Criar usuários: user-1, user-2 e user-3

Para cada usuário abaixo, siga os passos:

#### ➕ Criando `user-1`

- No menu lateral, clique em **Usuários** > **Adicionar usuários**
- Nome do usuário: `user-1`
- Tipo de acesso: ✅ Acesso ao Console da AWS
- Senha personalizada: `Lab-Password1`
- 🔄 Desmarque “Usuário precisa alterar a senha no próximo login”
- Clique em **Próximo: Permissões**
- Selecione: ❌ **Não adicionar permissões por enquanto**
- Clique em **Próximo** até finalizar a criação

#### 🔁 Repita o mesmo processo para:

- `user-2` com senha `Lab-Password2`
- `user-3` com senha `Lab-Password3`

---


![image (28)](https://github.com/user-attachments/assets/4e743341-985b-443a-8b8a-64393434c466)

![image (29)](https://github.com/user-attachments/assets/3626eb69-bc01-4d08-a84f-833186ed8bf1)

---
## 🛡️ ETAPA 2 – Criar Grupos IAM e Políticas

### ➕ Criar Grupos

1. No menu lateral, clique em **Grupos de usuários** > **Criar grupo**
2. Crie os grupos conforme abaixo:

| Grupo         | Política a anexar             | Tipo                   |
|---------------|-------------------------------|------------------------|
| `S3-Support`  | `AmazonS3ReadOnlyAccess`      | Política gerenciada AWS |
| `EC2-Support` | `AmazonEC2ReadOnlyAccess`     | Política gerenciada AWS |
| `EC2-Admin`   | (política inline customizada) | Política personalizada  |

> ⚠️ No grupo `EC2-Admin`, não anexe nenhuma política nesse momento — ela será criada manualmente como política inline.

---

![image (32)](https://github.com/user-attachments/assets/8d8fb926-0330-47c2-929f-a8102ea8f63a)

S3-Support

![image (33)](https://github.com/user-attachments/assets/9ccb9dda-83d7-4b0a-8a3f-648ff0b2af42) 

EC2-Support

EC2-Admin política nesse momento — ela será criada manualmente como política inline.

![image (34)](https://github.com/user-attachments/assets/888a38aa-58d7-47db-9a9f-587473ea4b84) 

---

## 👥 ETAPA 3 – Adicionar Usuários aos Grupos

Para cada grupo:

1. Vá até **Grupos de usuários**
2. Clique no grupo (ex: `S3-Support`)
3. Vá na aba **Usuários** > **Adicionar usuários**
4. Selecione o(s) usuário(s) indicados abaixo e clique em **Adicionar usuários**

| Grupo         | Usuário a adicionar |
|---------------|---------------------|
| S3-Support     | user-1              |
| EC2-Support    | user-2              |
| EC2-Admin      | user-3              |

---

| S3-Support     | user-1              |

![image (36)](https://github.com/user-attachments/assets/972fa453-b192-46e6-979f-0a5fd3710eed)

| EC2-Support    | user-2              |

![image (37)](https://github.com/user-attachments/assets/4c6d0232-66b8-4b69-880e-dd48ac32914e)

| EC2-Admin      | user-3              |

![image (39)](https://github.com/user-attachments/assets/22036399-05a6-4aeb-9aad-ca2e97d9af9e)

User groups (3)

![image](https://github.com/user-attachments/assets/494b9224-e826-49c2-a54b-16c0bdd15d0d)

## 🛠️ ETAPA 4 – Criar Política Inline no Grupo EC2-Admin

1. Acesse o grupo **EC2-Admin**
2. Vá na aba **Permissões** > **Adicionar política inline**
3. Selecione **Editor JSON** e cole o seguinte:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:RunInstances",
        "ec2:CreateSecurityGroup",
        "ec2:DescribeImages",
        "ec2:DescribeInstanceTypes",
        "ec2:CreateTags",
        "ec2:DescribeKeyPairs",
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeNetworkInterfaces"
      ],
      "Resource": "*"
    }
  ]
}

````

Clique em Revisar política

Nome da política: `EC2AdminInlinePolicy`

Clique em Criar política

---

## 🧪 ETAPA 5 – Testar Permissões
Copie o link de login para usuários IAM no painel inicial do IAM

Exemplo: `https://<account-id>.signin.aws.amazon.com/console`

Acesse esse link em uma janela anônima do navegador

### Testes:
🔸 user-1
Login com user-1 / Lab-Password1

Acessar o serviço S3 → deve conseguir visualizar buckets

![image (41)](https://github.com/user-attachments/assets/edeff6cb-78e7-4ad0-ace2-98895823e800)


Acessar o serviço EC2 → deve receber mensagem de "Não autorizado"

![image (53)](https://github.com/user-attachments/assets/8cf98923-4eb2-433d-93a7-afaa1512ea40)


🔸 user-2
Login com user-2 / Lab-Password2

Acessar EC2 → visualizar instâncias, mas não pode iniciar/parar 

![image (43)](https://github.com/user-attachments/assets/3fc0fa13-cd30-4929-9f6b-a2c7290cb344)

![image (47)](https://github.com/user-attachments/assets/bced267e-7852-471d-86d4-3754a0ba8aa5)



Acessar S3 → deve receber "Não autorizado"

![image (44)](https://github.com/user-attachments/assets/1e57fea1-dc6a-4df4-bff8-7950cafab7d4)


🔸 user-3
Login com user-3 / Lab-Password3

Acessar EC2 → pode visualizar, iniciar e parar instâncias 

![image](https://github.com/user-attachments/assets/605c56ec-bbdd-48da-b5a1-1d8873cc0759)

![image (48)](https://github.com/user-attachments/assets/551c02b3-b2ce-4e48-be74-f9f83a80dff8)

Instance State > Stop

![image (49)](https://github.com/user-attachments/assets/8212a727-26ee-4551-9427-2702b3f38fa5)

![image (50)](https://github.com/user-attachments/assets/87ce77b4-1c91-456f-8f1b-16f138c0216b)

Terminate (delete) instance 

![image (51)](https://github.com/user-attachments/assets/09e1febf-1b43-4e1b-84ef-ae44057a1882)

Você receberá o seguinte aviso 

Essa mensagem de erro significa que o usuário user-3 não tem permissão para executar a ação ec2:TerminateInstances, ou seja, não pode excluir (terminar) instâncias EC2.
![image (52)](https://github.com/user-attachments/assets/f7943338-2ba2-49bf-a813-ead67bf7180e)

## 📷 ETAPA 6 – Registrar Evidências
Durante os testes, tire prints de tela como evidência de:

### Acesso permitido e negado conforme as permissões

### Grupos e permissões associadas no IAM

### Console do usuário com ações visíveis

## ✅ Conclusão
Neste desafio, você praticou:

### Criação de usuários e grupos IAM

### Aplicação de políticas gerenciadas e inline

### Controle de acesso com base em função

### Testes práticos com login de usuários IAM

## 👍 Parabéns! Você concluiu com sucesso o Laboratório desafio aws iam run as cloud da comunidade Run as Cloud!

## ⚠️ Observações Finais
### Para evitar cobranças indevidas em sua conta AWS, é fundamental limpar todos os recursos criados durante o desafio após concluir a atividade.

🧹 Recursos que você deve excluir:
👥 Usuários IAM: user-1, user-2, user-3

👨‍👩‍👧‍👦 Grupos IAM: S3-Support, EC2-Support, EC2-Admin

📜 Políticas em linha, se aplicáveis

💻 Instâncias EC2 (como a instância LabHost usada nos testes)

📦 Buckets S3, se foram criados (mesmo que estejam vazios)

## ⚠️ Após a exclusão, revise o Painel de Faturamento da AWS e o AWS Cost Explorer para garantir que não há recursos ativos gerando custos.

## 📢 Compartilhe seu progresso!
### Marque a comunidade Run as Cloud no LinkedIn https://www.linkedin.com/company/run-as-cloud/?viewAsMember=true

Também marque os organizadores Grupos de Estudos :

Heberton Geovane https://www.linkedin.com/in/heberton-geovane/

Maik Biazi https://www.linkedin.com/in/maik-biazi-47b9291a5/

Michel Ernesto https://www.linkedin.com/in/mernesto/
