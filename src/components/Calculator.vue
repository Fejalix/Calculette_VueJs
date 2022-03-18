<template>
  <div class="p-3" style="max-width: 400px; margin: 50px auto; background: #234">
    
    <!-- Résultat de la calculatrice -->
    <div class="w-full rounded m-1 p-3 text-right lead font-weight-bold text-white bg-vue-dark">
      {{ calculatorValue || 0 }}
    </div>

    <!-- Boutons de la calculatrice -->
    <div class="row no-gutters">
      <div class="col-3" v-for="n in calculatorElements" :key="n"><!-- Boucle for pour creer un bouton pour chaque élément dans le tableau calculatorElements -->
        <div class="lead text-white text-center m-1 py-3 bg-vue-dark rounded hover-class"
          :class="{'bg-vue-green': ['C','*','/','-','+','%','='].includes(n)}"
          @click="action(n)"><!-- on ajoute une class css sur les elements du tableau dans le quel
            on va verifier sir les valeur sont incluse dans le tableau n .include(n) si elle est pas incluse la class css ne s'applique pas + mise place d'evenment click qui appellera une action (action(n))  -->
          {{n}} <!-- appelle l'element n de la boucle for-->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  props: {
    msg: String
  },
  data() {// creation fonction data
    return {
      calculatorValue: '',
      calculatorElements: ['C','*','/','-',7,8,9,'+',4,5,6,'%',1,2,3,'=',0,'.'],
      operator: null,
      previousCalculatorValue: '',
    }
  },
  methods: {// Regroupe les fonctions
    action(n){//cree la fonction action
      /* ajouter une valeur */
      if(!isNaN(n) || n === '.'){//condition if qui verifient si la valeur du bouton de clic est une valeur numérique ou si c'est égale à un "."
        this.calculatorValue += n + '';// On ajoute la valeur du bouton dans la valeur affiché par la calculatrice. La chaine vide vide permet de convertir la valeur numérique en valeur de chaine
      }
      /* Réinitialisé les valeurs */
      if(n === 'C'){// Condition if qui verifie sir la valeur du bouton est égale à C
        this.calculatorValue = '';//Remplace la valeur par une chaine vide
      }
      /* Pourcentage */
      if(n === '%'){// Condition if qui verifie sir la valeur du bouton est égale à %
        this.calculatorValue = this.calculatorValue / 100 + '';// on divise par 100 la valeur de calculatorValue puis on la convertit en chaine 
      }
        /* Operators */
      if(['/','*','-','+'].includes(n)){
        this.operator = n;// definit la valeur de operator sur n
        this.previousCalculatorValue = this.calculatorValue;//on definit previousCalculatorValue
        this.calculatorValue = '';// reintialison la valeur de calculatorValue
      }
      /* Calculer du résultat à l'aide de la fonction eval*/
      if(n === '='){// si la valeur du bouton cliqué est égale à "="
        this.calculatorValue = eval(// fonction eval pour calculer la valeur de la chaine
          this.previousCalculatorValue + this.operator + this.calculatorValue//concatène tout les opérateur ensemble
        );
        this.previousCalculatorValue = '';// reintialison la valeur de previousCalculatorValue
        this.operator = null;// reintialison la valeur de operator
      }
    }
  }
}
</script>


<style scoped>
  .bg-vue-dark {
    background: #31475e;
  }
  .hover-class:hover {
    cursor: pointer;
    background: #3D5875;
  }
  .bg-vue-green {
    background: #3fb984;
  }
</style>