<template lang="pug">
body
    .pt-14.pl-20
        .wrapper.pl-6
            form
              .table-container
                  table.table-auto.w-full.border
                      thead.h-12
                          tr.border
                              th.pl-4.text-left  
                              th.text-left NAME
                              th.text-left EMPLOYEE ID
                              th.text-left SECTION
                              th.text-left ROLE
                              th.text-left ACTION
                      tbody.h-10
                          tr.border(v-for="item in items")
                              td.pl-4(data-th=" ").pl-4.p-1
                                img(:src="item.profileimg").object-cover.rounded-full.h-10.w-10 
                              td(data-th="NAME") {{ item.firstname }}
                              td(data-th="NAME") {{ item.employee_id }}
                              td(data-th="SECTION") {{ item.section }}
                              td(data-th="ROLE") {{ item.role }}
                              td(data-th="ACTION")
                                button(@click="deleteUser(item._id)" type="button" class="button text-center text-sm p-1 bg-red-main focus:bg-red-500 focus:ring-red-200 w-20 h-9 text-white rounded") Delete
            
</template>

<script lang="js">
import Buttonred from '../../components/Buttonred.vue'

export default {
	components: {
        Buttonred,
    },
    layout: "layoutadmin",
    data() {
      return{
        items:[],
      }
    },
    async mounted(){
      const res = await this.$axios.get(`http://localhost:3030/users`)
      this.items = res.data
    },
    methods: {
      async deleteUser(_id) {
        if(confirm(`Do you want to delete this user?`)) {
          const res = await this.$axios.delete(
            `http://localhost:3030/users/${_id}`
          )
          location.reload()
        }
      }
    }
}
</script>

<style lang="scss" scoped>
.wrapper {
  margin-top: 20px;
  margin-bottom: 80px;
}
form {
  background: #ffffff;
  padding-bottom: 45px;
  width: 1100px;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
}

@media screen and (max-width: 768px) {
  html,
  body {
    font-size: 12px;
  }
}

.popup {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  width: 100vw;
  height: 100vh;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 2;
  visibility: hidden;
  opacity: 0;
  transition: 0.64s ease-in-out;
  &-inner {
    position: relative;
    bottom: -100vw;
    right: -100vh;
    display: flex;
    align-items: center;
    max-width: 600px;
    max-height: 600px;
    width: 32%;
    height: 25%;
    background-color: #fff;
    transform: rotate(32deg);
    transition: 0.64s ease-in-out;
  }
  &:target {
    visibility: visible;
    opacity: 1;
    .popup-inner {
      bottom: 0;
      right: 0;
      transform: rotate(0);
    }
  }
  &__close {
    position: absolute;
    right: -1rem;
    top: -1rem;
    width: 3rem;
    height: 3rem;
    font-size: 0.875rem;
    font-weight: 300;
    border-radius: 100%;
    background-color: #0a0a0a;
    z-index: 4;
    color: #fff;
    line-height: 3rem;
    text-align: center;
  }
}

</style>
