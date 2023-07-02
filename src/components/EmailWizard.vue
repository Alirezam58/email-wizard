<template>
    <div class="email-wizard">
      <div class="sidebar">
        <h3>Content Types</h3>
        <div class="content-types">
          <div
            class="content-type"
            v-for="(contentType, index) in contentTypes"
            :key="index"
            :draggable="true"
            @dragstart="dragStart(contentType)"
          >
            {{ contentType }}
          </div>
        </div>
      </div>
      <div class="content-area" @dragover="dragOver" @drop="drop">
        <div v-for="(item, index) in items" :key="index">
          <div class="content-header" v-if="item.type === 'headline'">
            <span>Header</span>
            <h2>{{ item.content }}</h2>
          </div>
          <div class="content-text" v-else-if="item.type === 'text'">
            <h3>Text</h3>
            <textarea v-model="item.content" placeholder="Enter text"></textarea>
          </div>
          <div class="content-image" v-else-if="item.type === 'image'">
            <h3>Image</h3>
            <img :src="item.content" alt="Image">
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name:'EmailWizard',
    data() {
      return {
        contentTypes: ['text', 'image', 'headline'],
        items: []
      };
    },
    methods: {
      dragStart(contentType) {
        event.dataTransfer.setData('text/plain', contentType);
      },
      dragOver(event) {
        event.preventDefault();
      },
      drop(event) {
        event.preventDefault();
        const contentType = event.dataTransfer.getData('text/plain');
        const content = '';
        
        if (contentType === 'text') {
          this.items.push({ type: contentType, content });
        } else if (contentType === 'image') {
          const imageUrl = prompt('Enter the image URL:');
          if (imageUrl) {
            this.items.push({ type: contentType, content: imageUrl });
          }
        } else if (contentType === 'headline') {
          const headline = prompt('Enter the headline:');
          if (headline) {
            this.items.push({ type: contentType, content: headline });
          }
        }
      }
    }
  };
  </script>
  
  <style>
  .email-wizard {
    display: flex;
    height: 100vh;
  }
  
  .sidebar {
    width: 200px;
    background: #f0f0f0;
    padding: 20px;
  }
  
  .content-types {
    margin-top: 20px;
  }
  
  .content-type {
    margin-bottom: 10px;
    padding: 5px 0;
    border: 1px solid #0e0e0e;
    border-radius: 2px;
    cursor: move;
  }
  
  .content-area {
    flex: 1;
    padding: 20px;
    border: 1px dashed #ccc;
  }

  .content-header{
    width: 100%;
    height: 50px;
    text-align: center;
  }

  .content-image{
    width: 50%;
    float: left;
  }

  .content-text{
    width: 50%;
    float: right;
  }
  
  textarea {
    width: 100%;
    height: 100px;
  }
  
  img {
    width: 250px;
    height: 250px;
  }
  
  h2 {
    margin-top: 0;
  }
  </style>
  
  
  
  
  
  