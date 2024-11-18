<template>
	<main>
		<div class="containerCadastro">
			<div class="contentCadastro">
				<h1>Cadastre-se</h1>
				<input type="text" id="user" name="user" placeholder="user" v-model="user">
				<input type="text" id="senha" name="senha" placeholder="senha" v-model="senha">
				<button type="submit" @click="addUser">Cadastrar</button>
				<button type="submit" @click="openModalUsers">Abrir modal de todos os usuarios</button>
			</div>

			<div class="usersAdded" v-if="listUsers">
				<div v-for="(user, index) in users" :key="index" class="users">
					<h5> Usuario: {{user.user}}</h5>
					<h5> Senha: {{user.senha}}</h5>
				</div>
			</div>

		</div>
	</main>
</template>

<script>
import axios from 'axios';

export default {
	name: 'CadastroUsuario',
	data () {
		return {
			users: [],
			// POST PARA CRIAR E GET PARA CHAMAR
			user: '',
			senha: '',
			listUsers: false,
		}
	},
	methods: {
		addUser() {
			if (!this.user || !this.senha) {
				return alert('Usuario e senha obrigatorios');	
			}
			this.users.push({
				user: this.user,
				senha: this.senha
			})
			axios.post('http://localhost:3003/cadastro', this.users)

			this.user = '';
			this.senha = '';

			// axios.get('http://localhost:3003/cadastro')
			// 	.then((response) => {
			// 		console.log(response.data);
			// 	})
			// 	.catch((error) => {
			// 		console.log(error);
			// 	})
		},
		openModalUsers() {
			this.listUsers = !this.listUsers;

			axios.get('http://localhost:3003/cadastro')
				.then((response) => {
					this.users = response.data;
				})
				.catch((error) => {
					console.log(error);
				})
			
		}
	},
}
</script>

<style scoped>
	main {
		width: 100%;
		height: 100vh;
	}
	.contentCadastro input{
		margin-left: 10px ;
	}

	.contentCadastro button{
		margin-left: 10px ;
	}
	
	.usersAdd {
		width: 100%;
		display: flex;
		justify-content: center;
		flex-direction: column;
		margin-top: 15px ;
	}

	.users {
		margin-top: 14px ;
		display: flex;
		flex-direction: column;
		width: 42%;
		margin-left: 320px;
		align-items: flex-start;
		background: #f4f4f4;
	}
	
	.listAllUsers{
		border: 1px solid red;
		height: 100%;
		width: 40%;
	}
</style>
