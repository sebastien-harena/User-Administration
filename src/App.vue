<template>
  <div class="container">

    <h1>Gestion Utilisateurs</h1>
    
    <!-- Contrôles -->
    <div class="controls">
      <button @click="toggleSelectionMode">
        {{ selectionMode ? 'Désactiver' : 'Activer' }} la sélection
      </button>
      <button @click="clearSelection" :disabled="selectedUsers.length === 0">
        Vider la sélection ({{ selectedUsers.length }})
      </button>
    </div>

    <!-- Grille d'utilisateurs -->
    <div class="users-grid">
      <Card
        v-for="user in users"
        :key="user.id"
        :user="user"
        :is-selectable="selectionMode"
        :action-text="getActionText(user)"
        :initial-selected="isUserSelected(user)"
        @user-selected="handleUserSelection"
        @action-clicked="handleUserAction"
      />
    </div>

    <!-- Détails de la sélection -->
    <div v-if="selectedUsers.length > 0" class="selection-details">
      <h3>Utilisateurs Sélectionnés</h3>
      <ul>
        <li v-for="user in selectedUsers" :key="user.id">
          {{ user.name }} - {{ user.role }}
          <button @click="deselectUser(user)">×</button>
        </li>
      </ul>
    </div>

    <!-- Dernière action -->
    <div v-if="lastAction" class="last-action">
      <strong>Dernière action :</strong> {{ lastAction }}
    </div>
  </div>
</template>

<script>
  import Card from './components/Card.vue'

  export default {
    name: 'UsersDashboard',
    components: {
      Card
    },
    data() {
      return {
        selectionMode: false,
        selectedUsers: [],
        lastAction: null,
        users: [
          {
            id: 1,
            name: 'Alice Johnson',
            email: 'alice@company.com',
            role: 'Développeuse Frontend',
            age: 28,
            location: 'Paris, France',
            isOnline: true,
            isVerified: true,
            avatar: 'https://i.pravatar.cc/150?img=1',
            skills: ['Vue.js', 'JavaScript', 'CSS', 'UI/UX']
          },
          {
            id: 2,
            name: 'Bob Smith',
            email: 'bob@company.com',
            role: 'Backend Developer',
            age: 32,
            location: 'Lyon, France',
            isOnline: false,
            isVerified: false,
            avatar: 'https://i.pravatar.cc/150?img=2',
            skills: ['Node.js', 'Python', 'SQL', 'AWS']
          },
          {
            id: 3,
            name: 'Carol Davis',
            email: 'carol@company.com',
            role: 'Product Manager',
            age: 35,
            location: 'Marseille, France',
            isOnline: true,
            isVerified: true,
            avatar: 'https://i.pravatar.cc/150?img=3',
            skills: ['Agile', 'Scrum', 'Product Strategy']
          },
          {
            id: 4,
            name: 'David Wilson',
            email: 'david@company.com',
            role: 'DevOps Engineer',
            age: 29,
            location: 'Toulouse, France',
            isOnline: true,
            isVerified: true,
            avatar: 'https://i.pravatar.cc/150?img=4',
            skills: ['Docker', 'Kubernetes', 'CI/CD', 'Linux']
          }
        ]
      }
    },
    methods: {
      toggleSelectionMode() {
        this.selectionMode = !this.selectionMode
        if (!this.selectionMode) {
          this.selectedUsers = []
        }
      },
      handleUserSelection({ user, selected }) {
        if (selected) {
          this.selectedUsers.push(user)
        } else {
          this.selectedUsers = this.selectedUsers.filter(u => u.id !== user.id)
        }
        this.lastAction = `${selected ? 'Sélection' : 'Désélection'} de ${user.name}`
      },
      handleUserAction(user) {
        this.lastAction = `Action sur ${user.name} (${user.role})`
        alert(`Action déclenchée pour : ${user.name}`)
      },
      getActionText(user) {
        return user.role.includes('Manager') ? 'Contacter' : 'Voir profil'
      },
      isUserSelected(user) {
        return this.selectedUsers.some(u => u.id === user.id)
      },
      deselectUser(user) {
        this.selectedUsers = this.selectedUsers.filter(u => u.id !== user.id)
      },
      clearSelection() {
        this.selectedUsers = []
        this.lastAction = 'Sélection vidée'
      }
    }
  }
</script>

<style scoped>

  .container {
    max-width: 1180px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'Arial', sans-serif;
  }


  h1 {
    background: linear-gradient(45deg, #ff6b6b, #feca57); /* #3498db; */
    color: white;
    padding: 25px 50px;
    font-size: 2.2rem;
    font-weight: bold;
    text-align: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    
    /* Clip-path sinusoïdal */
    clip-path: polygon(
      0% 10%,
      5% 5%,
      10% 10%,
      15% 5%,
      20% 10%,
      25% 5%,
      30% 10%,
      35% 5%,
      40% 10%,
      45% 5%,
      50% 10%,
      55% 5%,
      60% 10%,
      65% 5%,
      70% 10%,
      75% 5%,
      80% 10%,
      85% 5%,
      90% 10%,
      95% 5%,
      100% 10%,
      100% 90%,
      95% 95%,
      90% 90%,
      85% 95%,
      80% 90%,
      75% 95%,
      70% 90%,
      65% 95%,
      60% 90%,
      55% 95%,
      50% 90%,
      45% 95%,
      40% 90%,
      35% 95%,
      30% 90%,
      25% 95%,
      20% 90%,
      15% 95%,
      10% 90%,
      5% 95%,
      0% 90%
    );
    
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);


  }
  .controls {
    margin: 20px 0;
    padding: 15px;
    background: #f8f9fa;
    border-radius: 8px;
    display: flex;
    gap: 10px;
  }

  .controls button {
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 0.9em;
  }

  .controls button:first-child {
    background: #3498db;
    color: white;
  }

  .controls button:last-child {
    background: #e74c3c;
    color: white;
  }

  .controls button:disabled {
    background: #bdc3c7;
    cursor: not-allowed;
  }

  .users-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 20px;
    margin: 20px 0;
  }

  .selection-details {
    margin: 30px 0;
    padding: 20px;
    background: #e8f4fd;
    border-radius: 8px;
    border-left: 4px solid #3498db;
  }

  .selection-details ul {
    list-style: none;
    padding: 0;
  }

  .selection-details li {
    padding: 8px;
    margin: 5px 0;
    background: white;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .selection-details button {
    background: #e74c3c;
    color: white;
    border: none;
    border-radius: 50%;
    width: 25px;
    height: 25px;
    cursor: pointer;
  }

  .last-action {
    padding: 15px;
    background: #fff3cd;
    border: 1px solid #ffeaa7;
    border-radius: 5px;
    color: #856404;
  }
</style>