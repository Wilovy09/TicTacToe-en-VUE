<template>
    <div class="contenedor">
        <h1>Tic Tac Toe</h1>
        <div class="fila" v-for="(fila, i) in tablero" :key="i">
            <Casilla 
				v-for="(columna, j) in fila" 
				:figura="columna" 
				:key="j"
				@click="cambiarEstadoCasilla(i, j)"
				/>
        </div>
		<div class="fila">
			
			<button class="btn" 
					@click="resetear()"
					>
					Resetear
			</button>
			
			<button class="btn" 
					:class="{'btn-activo': turno === figura.X}"
					@click="cambiarTurno(figura.X)"
					>
					{{ figura.X }}
			</button>
			
			<button class="btn" 
					:class="{'btn-activo': turno === figura.O}"
					@click="cambiarTurno(figura.O)"
					>
					{{ figura.O }}
			</button>
		
		</div>
    </div>
</template>

<!-- TS -->
<script setup lang="ts">
	import { ref } from 'vue'
	import Casilla from './components/Casilla.vue';
	import { computed } from '@vue/reactivity';
	
	enum figura {
		X = '❌',
		O = '🔵'
	}

	const tablero = ref([
		['', '', ''],
		['', '', ''],
		['', '', '']
	])

	const resetear = ()=>{
		tablero.value = [
			['', '', ''],
			['', '', ''],
			['', '', '']
		]
	}

	const cambiarTurno = (figura:figura)=>{
		if(tableroVacio.value){
			turno.value = figura
		}
	}

	const tableroVacio = computed(()=>{
		for(let fila of tablero.value){
			for(let columna of fila){
				if(columna !== ''){
					return false
				}
			}
		}
		return true
	})

	const tableroLleno = computed(()=>{
		for(let fila of tablero.value){
			for(let columna of fila){
				if(columna === ''){
					return false
				}
			}
		}
		return true
	})

	const turno = ref(figura.X)

	const cambiarEstadoCasilla = (fila:number, columna:number)=>{
		if(!tablero.value[fila][columna]){
			tablero.value[fila][columna] = turno.value;
			turno.value = figura.X === turno.value ? figura.O : figura.X;
			
			// Si gano verticalmente
			if(verificarVertical(columna, tablero.value[fila][columna])){
				const ganador = tablero.value[fila][columna]
				alert('Gano: ' + ganador);
			}

			// Si gano horizontalmente
			if(verificarHorizontal(fila, tablero.value[fila][columna])){
				const ganador = tablero.value[fila][columna]
				alert('Gano: ' + ganador);
			}
			
			// Si gano diagonalmente
			if(verificarDiagonal(tablero.value[fila][columna])){
				const ganador = tablero.value[fila][columna]
				alert('Gano: ' + ganador);
			}

			// Empate
			if(tableroLleno.value){
				alert('Empate')
			}
		}
	}

	const verificarVertical = (columna: number, figura:string)=>{
		for(let i = 0; i < tablero.value.length; i++){
			if(tablero.value[i][columna] !== figura){
				return false
			}
		}
		return true
	}

	const verificarHorizontal = (fila: number, figura:string)=>{
		for(let i = 0; i < tablero.value[fila].length; i++){
			if(tablero.value[fila][i] !== figura){
				return false
			}
		}
		return true
	}

	const verificarDiagonal = (figura:string)=>{
		for(let i = 0; i < tablero.value.length; i++){
			if(tablero.value[i][i] !== figura){
				return false
			}
		}
		return true
	}

</script>

<!-- CSS -->
<style scoped>
	.contenedor{
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.fila{
		width: 350px;
		display: flex;
	}
	
	.btn {
		width: 100px;
		font-size: 20px;
		background-color: transparent;
		border: 1px solid #42B883;
		margin: 30px 5px;
		color: #FFF;
		cursor: pointer;
	}
	.btn-activo{
		background-color: #42B883;
	}
</style>
