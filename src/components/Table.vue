 <template>
  <v-container fluid>
    <v-data-iterator
      :items="items"
      :items-per-page.sync="itemsPerPage"
      :page="page"
      :search="search"
      :sort-by="sortBy.toLowerCase()"
      :sort-desc="sortDesc"
      hide-default-footer
    >
      <template v-slot:header>
        <v-toolbar
          dark
          color="blue darken-3"
          class="mb-1"
        >
          <v-text-field
            v-model="search"
            clearable
            flat
            solo-inverted
            hide-details
            prepend-inner-icon="mdi-magnify"
            label="Search"
          ></v-text-field>
          <template v-if="$vuetify.breakpoint.mdAndUp">
            <v-spacer></v-spacer>
            <v-select
              v-model="sortBy"
              flat
              solo-inverted
              hide-details
              :items="keys"
              prepend-inner-icon="mdi-sort"
              label="Sort by"
              @click="changeSort($event)"
            ></v-select>
            <v-spacer></v-spacer>
            <v-btn-toggle
              v-model="sortDesc"
              mandatory
            >
              <v-btn
                large
                depressed
                color="blue"
                :value="false"
              >
                <v-icon>mdi-arrow-up</v-icon>
              </v-btn>
              <v-btn
                large
                depressed
                color="blue"
                :value="true"
              >
                <v-icon>mdi-arrow-down</v-icon>
              </v-btn>
            </v-btn-toggle>
          </template>
        </v-toolbar>
        <v-toolbar
          dark
          color="blue darken-3"
          class="mb-1"
          v-if="!$vuetify.breakpoint.mdAndUp"
        >
          <template>
            <v-select
              v-model="sortBy"
              flat
              solo-inverted
              hide-details
              :items="keys"
              prepend-inner-icon="mdi-sort"
              label="Sort by"
            ></v-select>
          </template>
        </v-toolbar>
        <v-toolbar
          dark
          color="blue darken-3"
          class="mb-1"
          v-if="!$vuetify.breakpoint.mdAndUp"
        >
          <template>
            <v-btn-toggle
              v-model="sortDesc"
              mandatory
            >
              <v-btn
                large
                depressed
                color="blue"
                :value="false"
              >
                <v-icon>mdi-arrow-up</v-icon>
              </v-btn>
              <v-btn
                large
                depressed
                color="blue"
                :value="true"
              >
                <v-icon>mdi-arrow-down</v-icon>
              </v-btn>
            </v-btn-toggle>
          </template>
        </v-toolbar>
        <v-toolbar>
          Total Cases Confirmed: {{total_confirmed}}<br/>
          Total Active Cases: {{total_active}}
        </v-toolbar>
      </template>
      <template v-slot:default="props">
        <v-row align="stretch">
          <v-col
            v-for="item in props.items"
            :key="item.name"
            cols="12"
            sm="6"
            md="4"
            lg="3"
          >
            <v-card>
              <v-card-title class="subheading font-weight-bold">{{ item.name }}</v-card-title>

              <v-divider></v-divider>

              <v-list dense>
                <v-list-item
                  v-for="(key, index) in filteredKeys"
                  :key="index"
                >
                  <v-list-item-content :class="{ 'blue--text': sortBy === key }">{{ key }}:</v-list-item-content>
                  <v-list-item-content class="align-end" :class="{ 'blue--text': sortBy === key }">{{ item[key.toLowerCase()] }}</v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </template>

      <template v-slot:footer>
        <v-row class="mt-2" align="center" justify="center">
          <span class="grey--text">Items per page</span>
          <v-menu offset-y>
            <template v-slot:activator="{ on }">
              <v-btn
                dark
                text
                color="primary"
                class="ml-2"
                v-on="on"
              >
                {{ itemsPerPage }}
                <v-icon>mdi-chevron-down</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item
                v-for="(number, index) in itemsPerPageArray"
                :key="index"
                @click="updateItemsPerPage(number)"
              >
                <v-list-item-title>{{ number }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>

          <v-spacer></v-spacer>

          <span
            class="mr-4
            grey--text"
          >
            Page {{ page }} of {{ numberOfPages }}
          </span>
          <v-btn
            fab
            dark
            color="blue darken-3"
            class="mr-1"
            @click="formerPage"
          >
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
          <v-btn
            fab
            dark
            color="blue darken-3"
            class="ml-1"
            @click="nextPage"
          >
            <v-icon>mdi-chevron-right</v-icon>
          </v-btn>
        </v-row>
      </template>
    </v-data-iterator>
  </v-container>
</template>


<script>
  export default {
    data () {
      return {
        itemsPerPageArray: [4, 8, 12,24],
        search: '',
        filter: {},
        sortDesc: true,
        page: 1,
        itemsPerPage: 12,
        sortBy: 'active',
        keys: [
          'name',
          'active',
          'confirmed',
          'deceased',
          'recovered',
        ],
        items:[],
        total_confirmed:0,
        total_active:0,
      }
    },
    computed: {
      numberOfPages () {
        return Math.ceil(this.items.length / this.itemsPerPage)
      },
      filteredKeys () {
        return this.keys.filter(key => key !== `Name`)
      },
    },
    methods: {
      nextPage () {
        if (this.page + 1 <= this.numberOfPages) {
          this.scrollToTop();
          this.page += 1
        }
      },
      formerPage () {
        if (this.page - 1 >= 1) {
          this.scrollToTop();
          this.page -= 1
        }
      },
      updateItemsPerPage (number) {
        this.itemsPerPage = number
      },
      // checkCorrectData(item){
      //   if(item.name==='Unknown')return false;
      //   return true;
      // },
      // cleanData(items) { 
      //   this.items = items.filter(function(item) {
      //   const flag = this.checkCorrectData(item);
      //   return flag===true;
      // });
      // console.log(this.items);
      // },
      formData(data){
        for (let [key, value] of Object.entries(data)) {
          const element = {};
          element.name = key;
          element.active = value.active;
          element.confirmed = value.confirmed;
          element.deceased = value.deceased;
          element.recovered = value.recovered;
          this.total_active = this.total_active + parseInt(element.active);
          this.total_confirmed = this.total_confirmed + parseInt(element.confirmed);
          if(element.name==='Unknown') continue;
          if(element.name==='Other State') continue;
          if(element.name==='Foreign Evacuees') continue;
          this.items.push(element);
      }
      // this.cleanData(this.items);
      },
      changeSort($event) {
        console.log($event);
      },
      scrollToTop(){
        this.$vuetify.goTo(0, { duration: 1000 });
        window.scrollTo(0, 0);
      }
    },
    mounted() {
    var requestOptions = {
    method: 'GET',
    redirect: 'follow'
    };

    fetch("https://api.covid19india.org/state_district_wise.json", requestOptions)
    .then(response => response.json())
    .then(result => {
        const data = result.Telangana.districtData;
        this.formData(data);
    })
    .catch(error => console.log('error', error));
    }
}
</script>

<style type="text/css">

</style>