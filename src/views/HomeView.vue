<template>
  <v-container>
    <v-row justify="center">
      <!-- Account List Section -->
      <v-col cols="12" lg="8">
        <v-card class="elevation-2 mb-4">
          <v-card-title>
            <span class="headline text-primary">Account List</span>
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="list in lists" :key="list.list_id" cols="12" sm="6" md="4">
                <v-card class="elevation-1 account-card">
                  <v-card-title class="text-h6">{{ list.name }}</v-card-title>
                  <v-card-subtitle class="text-caption text-grey"
                    >Balance: {{ list.balance }}</v-card-subtitle
                  >
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Account Statistics Section -->
      <v-col cols="12" lg="8">
        <v-card class="elevation-2 mt-4">
          <v-card-title>
            <span class="headline text-success">Account Statistics</span>
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="statistics in stats" :key="statistics.stats_id" cols="12" sm="6" md="4">
                <v-card class="elevation-1 stat-card">
                  <v-card-title class="text-h6">
                    <v-icon class="mr-2 text-primary">mdi-account-group</v-icon>
                    Total Users: {{ statistics.total_user }}
                  </v-card-title>
                  <v-card-subtitle class="text-caption text-grey">
                    <v-icon class="mr-2 text-success">mdi-currency-usd</v-icon>
                    Total Balance: {{ statistics.total_balance }}
                  </v-card-subtitle>
                  <v-card-subtitle class="text-caption text-grey">
                    <v-icon class="mr-2 text-warning">mdi-chart-bar</v-icon>
                    Average Balance: {{ statistics.average_balance }}
                  </v-card-subtitle>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useUserStore } from '@/stores/user'

const stats = ref([]) // Array to store account statistics
const lists = ref([]) // Array to store account lists
const userStore = useUserStore()

// Function to fetch account statistics
const fetchStats = async () => {
  try {
    const response = await axios.get('http://localhost:8081/account/statistics', {
      headers: {
        Authorization: userStore.token,
      },
    })
    stats.value = response.data
  } catch (error) {
    console.error('Error fetching stats:', error)
  }
}

// Function to fetch account list
const fetchList = async () => {
  try {
    const response = await axios.get('http://localhost:8081/account/list')
    lists.value = response.data.data
  } catch (error) {
    console.error('Error fetching lists:', error)
  }
}

// Fetch data on mount
onMounted(() => {
  fetchStats()
  fetchList()
})
</script>

<style scoped>
.account-card {
  padding: 16px;
  border-radius: 8px;
  background: #f7f9fc;
  color: #333;
}

.stat-card {
  padding: 16px;
  border-radius: 8px;
  background: #e8f5e9;
  color: #2e7d32;
}

.text-primary {
  color: #1976d2 !important;
}

.text-success {
  color: #2e7d32 !important;
}

.text-warning {
  color: #f9a825 !important;
}
</style>
