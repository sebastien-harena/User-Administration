<template>
  <div 
    class="user-card" 
    :class="{ 
      'selected': isSelected, 
      'selectable': isSelectable,
      'online': user.isOnline 
    }"
    @click="handleCardClick"
  >
    <!-- En-tête avec badge de statut -->
    <div class="card-header">
      <div class="status-indicator" :class="user.isOnline ? 'online' : 'offline'"></div>
      <span class="user-name">{{ user.name }}</span>
      <span class="user-age">{{ user.age }} ans</span>
    </div>

    <!-- Avatar -->
    <div class="avatar-section">
      <img 
        :src="user.avatar" 
        :alt="user.name" 
        class="user-avatar"
      />
      <div v-if="user.isVerified" class="verified-badge">✓</div>
    </div>

    <!-- Informations utilisateur -->
    <div class="user-info">
      <p class="user-email">
        <span class="icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M4 4H20C21.1 4 22 4.9 22 6V18C22 19.1 21.1 20 20 20H4C2.9 20 2 19.1 2 18V6C2 4.9 2.9 4 4 4Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M22 6L12 13L2 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
        </span>
        {{ user.email }}
      </p>
      <p class="user-role">
        <span class="icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect x="1" y="5" width="20" height="15" rx="1" fill="#AB70DA"/>
                <rect x="1" y="5" width="20" height="3" fill="#BA55D3"/>
                <rect x="8" y="5" width="6" height="1.5" rx="0.75" fill="#D8BFD8"/>
                <circle cx="7.5" cy="10" r="0.6" fill="#FFD700"/>
                <circle cx="16.5" cy="10" r="0.6" fill="#FFD700"/>
            </svg>
        </span>
        {{ user.role }}
      </p>
      <p class="user-location">
        <span class="icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M12 2C8.134 2 5 5.134 5 9C5 13.06 8.327 17.443 12 21C15.673 17.443 19 13.06 19 9C19 5.134 15.866 2 12 2Z" fill="#FF7F50"/>
                <path d="M12 12C13.1046 12 14 11.1046 14 10C14 8.89543 13.1046 8 12 8C10.8954 8 10 8.89543 10 10C10 11.1046 10.8954 12 12 12Z" fill="white"/>
                <path d="M12 2C8.134 2 5 5.134 5 9C5 13.06 8.327 17.443 12 21C15.673 17.443 19 13.06 19 9C19 5.134 15.866 2 12 2Z" stroke="#FF6347" stroke-width="1"/>
            </svg>
        </span>
        {{ user.location }}
      </p>
    </div>

    <!-- Compétences -->
    <div class="skills-section" v-if="user.skills && user.skills.length">
      <h4>Compétences</h4>
      <div class="skills-list">
        <span 
          v-for="skill in user.skills" 
          :key="skill"
          class="skill-tag"
        >
          {{ skill }}
        </span>
      </div>
    </div>

    <!-- Actions -->
    <div class="card-actions">
      <button 
        class="action-btn primary"
        @click.stop="handleActionClick"
      >
        {{ actionText }}
      </button>
      <button 
        v-if="isSelectable"
        class="action-btn secondary"
        @click.stop="toggleSelection"
      >
        {{ isSelected ? 'Désélectionner' : 'Sélectionner' }}
      </button>
    </div>

    <!-- Indicateur de sélection -->
    <div v-if="isSelected" class="selection-overlay">
      <div class="checkmark">✓</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  props: {
    user: {
      type: Object,
      required: true,
      validator: (user) => {
        return user.name && user.email && user.role
      }
    },
    isSelectable: {
      type: Boolean,
      default: false
    },
    actionText: {
      type: String,
      default: 'Action'
    },
    initialSelected: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isSelected: this.initialSelected
    }
  },
  emits: ['user-selected', 'action-clicked'],
  methods: {
    handleCardClick() {
      if (this.isSelectable) {
        this.toggleSelection()
      }
    },
    toggleSelection() {
      this.isSelected = !this.isSelected
      this.$emit('user-selected', {
        user: this.user,
        selected: this.isSelected
      })
    },
    handleActionClick() {
      this.$emit('action-clicked', this.user)
    }
  },
  watch: {
    initialSelected(newVal) {
      this.isSelected = newVal
    }
  }
}
</script>

<style scoped>
.user-card {
  position: relative;
  border: 2px solid #e0e0e0;
  border-radius: 15px;
  padding: 20px;
  margin: 10px;
  max-width: 320px;
  background: white;
  transition: all 0.3s ease;
  cursor: default;
}

.user-card.selectable {
  cursor: pointer;
}

.user-card.selectable:hover {
  border-color: #3498db;
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.user-card.selected {
  border-color: #2ecc71;
  background-color: #f8fff9;
}

.user-card.online {
  border-left: 4px solid #2ecc71;
}

/* En-tête */
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.status-indicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.status-indicator.online {
  background-color: #2ecc71;
}

.status-indicator.offline {
  background-color: #e74c3c;
}

.user-name {
  font-weight: bold;
  font-size: 1.2em;
  color: #2c3e50;
}

.user-age {
  color: #7f8c8d;
  font-size: 0.9em;
}

/* Avatar */
.avatar-section {
  position: relative;
  text-align: center;
  margin: 15px 0;
}

.user-avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #ecf0f1;
}

.verified-badge {
  position: absolute;
  bottom: 5px;
  right: calc(50% - 40px);
  background: #3498db;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Informations */
.user-info {
  margin: 15px 0;
}

.user-info p {
  margin: 8px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.icon {
  font-size: 0.9em;
}

.user-email {
  color: #3498db;
}

.user-role {
  color: #9b59b6;
}

.user-location {
  color: #e67e22;
}

/* Compétences */
.skills-section {
  margin: 15px 0;
}

.skills-section h4 {
  margin-bottom: 8px;
  color: #2c3e50;
  font-size: 0.9em;
}

.skills-list {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.skill-tag {
  background: #ecf0f1;
  padding: 3px 8px;
  border-radius: 12px;
  font-size: 0.8em;
  color: #7f8c8d;
}

/* Actions */
.card-actions {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.action-btn {
  flex: 1;
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9em;
  transition: background-color 0.2s;
}

.action-btn.primary {
  background-color: #3498db;
  color: white;
}

.action-btn.primary:hover {
  background-color: #2980b9;
}

.action-btn.secondary {
  background-color: #ecf0f1;
  color: #2c3e50;
}

.action-btn.secondary:hover {
  background-color: #bdc3c7;
}

/* Overlay de sélection */
.selection-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(46, 204, 113, 0.1);
  border-radius: 13px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.checkmark {
  background: #2ecc71;
  color: white;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5em;
  font-weight: bold;
}
</style>