<template>
  <div class="grid-container">
    <div class="grid-x">
      <div class="cell small-12">
        <h1>Phonebook</h1>
        <app-search @inputChanged="searchUpdate"></app-search>
        <div class="grid-x" v-show="filteredContacts.length > 0 ">
          <div class="cell small-12 contactsWrapper">
            <ul class="contacts">
              <li v-for="(contact,index) in filteredContacts" :key="index">
                <p class="contact">
                  <span class="fwb">Name:</span> {{contact.name}} <br/>
                  <span class="fwb">Address:</span> {{contact.address }}<br/>
                  <span class="fwb">Phone:</span> {{contact.phone_number}}
                </p>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//  Import the search component
import appSearch from './components/Search.vue';
//  Axios for the AJAX request
import axios from 'axios';

export default {
    data() {
        return {
            //  List of contacts
            //  Initially empty, populated by the AJAX request in the created() hook
            contacts: [],
            //  The raw search
            //  Received from the search component
            //  Not formatted, i.e may contain mixed uppercase and lowercase characters
            search: '',
        };
    },
    components: {
        // The search box component
        appSearch,
    },

    methods: {
        //  Called when the @inputChanged event is fired
        //  Is fired when the input changes in search component
        //  $event is equal to the input field's value
        searchUpdate($event) {
            this.search = $event;
        },
    },

    computed: {
        // This function is what populates the list of contacts initially shown
        // First it calls .filter() on each array item (object)
        // Searches the array item's name, address and phone_number
        // Converted to lowercase for more/less accurate searches
        // Returns true (if a match)
        // Returns false if no match to the filtered search(the search in lowercase)
        filteredContacts() {
            return this.contacts.filter(contact => {
                if (contact.name.toLowerCase().match(this.filteredSearch)) {
                    return true;
                } else if (
                    contact.phone_number
                        .toLowerCase()
                        .match(this.filteredSearch)
                ) {
                    return true;
                } else if (
                    contact.address.toLowerCase().match(this.filteredSearch)
                ) {
                    return true;
                } else {
                    return false;
                }
            });
        },
        // Converts the search held in the data() to lowercase
        filteredSearch() {
            return this.search.toLowerCase();
        },
    },
    // Lifecycle hook
    // When the instance is created calls for the contact list
    created() {
        // that = this so the callback can push into the correct array
        // "this" in this instance would not have the correct binding
        const that = this;
        axios
            .get('http://www.mocky.io/v2/581335f71000004204abaf83')
            .then(function(response) {
                // Handle success
                // Push the response data contacts into the contacts array...
                // held in data
                for (let i = 0; i < response.data.contacts.length; i++) {
                    that.contacts.push(response.data.contacts[i]);
                }
            })
            .catch(function(error) {
                // Handle error
                console.log(error);
            });
    },
};
</script>

<style lang="scss">
.app {
    margin: 1em auto 0;
    border: 2px solid #eee;
    border-radius: 4px;
    padding: 1em;
}

h1 {
    text-align: center;
}

.contacts {
    list-style: none;
}

.contactsWrapper {
    border: 2px solid #eee;
    border-radius: 4px;
    padding: 1em;
    margin-top: 2em;
    background: #fff;
}

.contact {
    font-size: 1.3em;
    margin: 0.5em 0;
}

.fwb {
    font-weight: 500;
}
</style>
