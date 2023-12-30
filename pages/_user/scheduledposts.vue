<template>
    <div class="login">
        <template>
          <div class="card mt-4">
            <div v-if="Object.keys(cronBlogs).length > 0" class="card-header">
              Schedules Booked
            </div>
            <div v-if="Object.keys(cronBlogs).length > 0" class="card-body">
              <div class="table-responsive">
                <table class="table table-striped">
                  <thead>
                    <tr>
                      <th id='date_th'>Date</th>
                      <th>Time</th>
                      <th>Post</th>
                      <th>Parent Permlink</th>
                      <th>Title</th>
                      <th>Permlink</th>
                      <th>Delete</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in cronBlogs" :key="item._id">
                      <td id='date_td'>{{ item.date }}</td>
                      <td>{{ item.time }}</td>
                      <td>{{ item.body }}</td>
                      <td>{{ item.parent_permlink }}</td>
                      <td>{{ item.title }}</td>
                      <td>{{ item.permlink }}</td>
                      <td>
                        <button @click.prevent="deleteBlog(item._id)" class="btn btn-danger">Delete</button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </template>
    </div>
  </template>
  
  
  <script>
  import axios from 'axios'
  import moment from 'moment'
  import postIndex from '@/mixins/postIndex'

  export default {
    mixins: [postIndex],

    data () {
      return {
        cronBlogs: {
          date: '',
          time: '',
          body: '',
          permlink: 'my-new-blog',
          parent_permlink:'blockchain',
          title: '',
          parent_author: '',
          json_metadata: ''
        }
      }
    },
    mounted() {
      this.fetchData();
    },
    methods: {
      async fetchData() {
        try {
          const response = await axios.get(process.env.FETCH);
          let data = response.data;
          this.cronBlogs = data.map(entry => ({
            ...entry,
            date: moment(entry.date).format('MMM DD, YYYY')
          }));
        } catch (error) {
          console.error('Failed to fetch data', error);
        }
      },

      async deleteBlog(id) {
        try {
          const response = await axios.delete(`${process.env.DELETE}/${id}`);
          await this.fetchData();
        } catch (error) {
          console.error('Failed to delete', error.response.data.error);
        }
      }

    }
  }
  </script>
  
  <style>
  #date_th, #date_td {
    width: 25% !important;
    white-space: nowrap;
  }
  
  tbody tr td {
   width: 5%;
   white-space: normal;
  }
  </style>
  