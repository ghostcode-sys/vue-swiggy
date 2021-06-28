  <template>
  <div class="page-container">
    <div class="sidebar">
      <div class="side-content">
        <div
          class="category"
          v-on:click="
            toggle(1);
            positionTag(0);
          "
          :class="tagged[0] && show ? 'tagged' : ''"
        >
          Popular Brands
          <div class="count-res">
            {{ popular.restaurantList.length }} Restaurants
          </div>
        </div>
        <div
          class="category"
          v-on:click="
            positionTag(1);
            toggle(1);
          "
          :class="tagged[1] && show ? 'tagged' : ''"
        >
          Offers Near You
          <div class="count-res">
            {{ offer.restaurantList.length }} Restaurants
          </div>
        </div>
        <div
          class="category"
          v-on:click="
            toggle(1);
            positionTag(2);
          "
          :class="tagged[2] && show ? 'tagged' : ''"
        >
          Express Delivery
          <div class="count-res">
            {{ express.restaurantList.length }} Restaurants
          </div>
        </div>
        <div
          class="category"
          v-on:click="
            toggle(1);
            positionTag(3);
          "
          :class="tagged[3] && show ? 'tagged' : ''"
        >
          Gourmet
          <div class="count-res">
            {{ gourmet.restaurantList.length }}Restaurants
          </div>
        </div>
        <div
          class="category"
          v-on:click="
            toggle(1);
            positionTag(4);
          "
          :class="tagged[4] && show ? 'tagged' : ''"
        >
          Only In Swiggy
          <div class="count-res">
            {{ exclusive.restaurantList.length }} Restaurants
          </div>
        </div>
        <div
          class="category"
          v-on:click="toggle(0)"
          :class="!show ? 'tagged' : ''"
        >
          See All
          <div class="count-res">
            {{
              popular.restaurantList.length +
              offer.restaurantList.length +
              express.restaurantList.length +
              gourmet.restaurantList.length
            }}
            Restaurants
          </div>
        </div>
      </div>
    </div>

    <div class="main-collection">
      <div
        class="category-collection"
        v-for="i in 5"
        v-bind:key="i"
        :class="show ? '' : 'n-display'"
      >
        <div class="collection-name" :id="nameId(i - 1)">
          <span>{{ collection[i - 1].category }}</span>
        </div>
        <div class="card-collection">
          <div
            class="card card-hover"
            v-for="(item, index) of collection[i - 1].restaurantList"
            :key="index"
          >
            <div :class="index > cardDisplay(i - 1) ? 'n-display' : ''">
              <img :src="images[index % 11]" />
              <div class="card-content">
                {{ item.name }}
                <div class="card-tags">
                  <div
                    class="card-tag"
                    v-for="(type, indexs) in item.food_types"
                    :key="indexs"
                  >
                    <span v-if="indexs < 3">{{ type }}</span>
                  </div>
                </div>
                <div class="card-values">
                  <div
                    class="card-rating"
                    :class="item.ratings === '' ? '' : 'green'"
                  >
                    <i class="fa fa-fw fa-star"></i>
                    <span v-if="item.ratings === ''"> -- </span
                    ><span v-else> {{ item.ratings }}</span>
                  </div>
                  <div class="dot"><i class="fa fa-fw fa-circle"></i></div>
                  <div class="card-delivery">{{ item.delivery_time }}</div>
                  <div class="dot"><i class="fa fa-fw fa-circle"></i></div>
                  <div class="card-value">{{ item.price_for_two }} for two</div>
                </div>
              </div>
              <div class="card-quick-view">QUICK VIEW</div>
            </div>
            <div
              :class="index == cardDisplay(i - 1) ? 'card-wrap' : 'n-display'"
              v-on:click="incrementCard(i - 1)"
            >
              +{{
                collection[i - 1].restaurantList.length - cardDisplay(i - 1)
              }}
              More
            </div>
          </div>
        </div>
      </div>
      <!-- see all section -->
      <div class="category-collection" :class="!show ? '' : 'n-display'">
        <div class="collection-name">All Restaurants</div>
        <div
          class="card-collection"
          v-for="(item, index) in value"
          v-bind:key="index"
        >
          <div
            class="card card-hover"
            v-for="(cards, index) of item.restaurantList"
            :key="index"
          >
            <div>
              <img :src="images[index % 11]" />
              <div class="card-content">
                {{ cards.name }}
                <div class="card-tags">
                  <div
                    class="card-tag"
                    v-for="(type, indexs) in cards.food_types"
                    :key="indexs"
                  >
                    <span v-if="indexs < 3">{{ type }}</span>
                  </div>
                </div>
                <div class="card-values">
                  <div
                    class="card-rating"
                    :class="cards.ratings === '' ? '' : 'green'"
                  >
                    <i class="fa fa-fw fa-star"></i>
                    <span v-if="cards.ratings === ''"> -- </span
                    ><span v-else> {{ cards.ratings }}</span>
                  </div>
                  <div class="dot"><i class="fa fa-fw fa-circle"></i></div>
                  <div class="card-delivery">{{ cards.delivery_time }}</div>
                  <div class="dot"><i class="fa fa-fw fa-circle"></i></div>
                  <div class="card-value">
                    {{ cards.price_for_two }} for two
                  </div>
                </div>
              </div>
              <div class="card-quick-view">QUICK VIEW</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
     name: 'Main',
  data() {
    return {
      images: [
        "https://images.unsplash.com/photo-1484723091739-30a097e8f929?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1521305916504-4a1121188589?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1511690743698-d9d85f2fbf38?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1506084868230-bb9d95c24759?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1429554513019-6c61c19ffb7e?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1482049016688-2d3e1b311543?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1496412705862-e0088f16f791?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1432139509613-5c4255815697?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1478145046317-39f10e56b5e9?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1484980972926-edee96e0960d?auto=format&fit=crop&w=500&q=60",
        "https://images.unsplash.com/photo-1504544750208-dc0358e63f7f?auto=format&fit=crop&w=500&q=60",
      ],
      value: [],
      popular: [],
      offer: [],
      express: [],
      gourmet: [],
      res: [],
      exclusive: { category: "Only In Swiggy", restaurantList: [] },
      popularlen: 5,
      offerlen: 5,
      expresslen: 5,
      gourmetlen: 5,
      exclusivelen: 5,
      collection: [],
      show: true,
      tagged: [],
    };
  },
  methods: {
    cardDisplay(index) {
      if (index == 0) {
        return this.popularlen;
      } else if (index == 1) {
        return this.offerlen;
      } else if (index == 2) {
        return this.expresslen;
      } else if (index == 3) {
        return this.gourmetlen;
      } else if (index == 4) {
        return this.exclusivelen;
      }
    },
    incrementCard(index) {
      if (index == 0) {
        this.popularlen += 6;
        if (this.popularlen > this.popular.restaurantList.length) {
          this.popularlen = this.popular.restaurantList.length;
        }
        return this.popularlen;
      } else if (index == 1) {
        this.offerlen += 6;
        if (this.offerlen > this.offer.restaurantList.length) {
          this.offerlen = this.offer.restaurantList.length;
        }
        return this.offerlen;
      } else if (index == 2) {
        this.expresslen += 6;
        if (this.expresslen > this.express.restaurantList.length) {
          this.expresslen = this.express.restaurantList.length;
        }
        return this.expresslen;
      } else if (index == 3) {
        this.gourmetlen += 6;
        if (this.gourmetlen > this.gourmet.restaurantList.length) {
          this.gourmetlen = this.gourmet.restaurantList.length;
        }
        return this.gourmetlen;
      } else if (index == 4) {
        this.exclusivelen += 6;
        if (this.exclusivelen > this.exclusive.restaurantList.length) {
          this.exclusivelen = this.exclusive.restaurantList.length;
        }
        return this.exclusivelen;
      }
    },
    toggle(num) {
      if (num == 1) {
        this.show = true;
      } else if (num == 0) {
        this.show = false;
      }
    },
    positionTag(index) {
      this.popularlen = 5;
      this.expresslen = 5;
      this.offerlen = 5;
      this.exclusivelen = 5;
      this.gourmetlen = 5;
      const id = document.getElementById(`category${index}`);
      // const id1 = document.getElementById(`category${index -1}`);
      id.scrollIntoView(true);
    },
    nameId(i) {
      return `category${i}`;
    },
    handleScroll() {
      this.tagged.length = 0;
      for (let i = 0; i < 5; i += 1) {
        const box = document.querySelector(`#category${i}`);
        const view = this.isInViewport(box);
        this.tagged.push(view);
      }
    },
    isInViewport(el) {
      const rect = el.getBoundingClientRect();
      return (
        rect.top >= 0 &&
        rect.left >= 0 &&
        rect.bottom <=
          (window.innerHeight || document.documentElement.clientHeight) &&
        rect.right <=
          (window.innerWidth || document.documentElement.clientWidth)
      );
    },
  },
  created() {
    window.addEventListener("scroll", this.handleScroll);
  },
  unmounted() {
    window.removeEventListener("scroll", this.handleScroll);
  },

  beforeMount() {
    console.log("mount is ok");
    fetch("https://mocki.io/v1/3fb1488d-bbdb-4ddd-9a03-a0d2efc98597").then(
      (res) => {
        res.json().then((data) => {
            this.value = data;
            // console.log("inside 2 thens ");
            // console.log("data-->",data);
            [this.popular, this.offer, this.express, this.gourmet] = data;
            data.map((val) => {
              val.restaurantList.map((restaurant) => {
                if (restaurant.isExlusive) {
                  this.res.push(restaurant);
                }
              });
            });
     for (let i of this.res) {
      this.exclusive.restaurantList.push(i);
    }
      this.collection = [
      this.popular,
      this.offer,
      this.express,
      this.gourmet,
      this.exclusive,
    ];
    
          })
      }
    ).catch((err) => {
            console.warn("error is here in parsing ",err);
          });
     
    // console.log(this.exclusive);
  
    console.log("collection --:> ",this.collection)
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  * {
  margin: 0;
  padding: 0;
}
html {
  scroll-behavior: smooth;
}
#app {
  font-family: ProximaNova, Arial, Helvetica Neue, sans-serif;
  text-align: center;
  margin-top: 100px;
}

.page-container {
  width: 100%;
  max-width: 1200px;
  margin: auto;
  padding: 0 32px;
  display: -webkit-flex;
  display: flex;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  position: relative;
}
.category-collection {
  margin-bottom: 100px;
}

.main-collection {
  max-width: calc(100% - 250px);
}

/* card container */
.card {
  width: 294px;
  box-sizing: border-box;
  margin: 8px;
  padding: 16px;
  cursor: pointer;
  background-color: #fff;
  position: relative;
}

.card img {
  width: 260px;
  height: 120px;
}

.card-content {
  margin-top: 8px;
  text-align: left;
}

.card-tags {
  display: -webkit-flex;
  display: flex;
  -webkit-align-cards: center;
  align-items: center;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
  margin-top: 4px;
}

.card-tag {
  padding: 2px 8px;
  background-color: #ececec;
  margin: 4px;
  font-size: 12px;
  border-radius: 20px;
  color: #373737;
  cursor: pointer;
  opacity: 0.6;
}

.card-values {
  display: -webkit-flex;
  display: flex;
  -webkit-align-items: center;
  align-items: center;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  font-size: 12px;
  color: #979797;
  margin-top: 16px;
}

.card-rating {
  padding: 2px 6px;
  border-radius: 2px;
}

.card-rating.green {
  background-color: #48c479;
  color: #fff;
}

.dot {
  font-size: 5px;
}

.card-quick-view {
  font-size: 14px;
  border-top: 1px solid #e9e9eb;
  padding-top: 14px;
  margin-top: 14px;
  font-weight: 500;
  text-align: center;
  color: #5d8ed5;
  opacity: 0;
}

.card-hover:hover > .card-quick-view {
  opacity: 1;
}

.card-hover:hover {
  border: 1px solid #e9e9eb;
}

/* side bar */
.sidebar {
  width: 250px;
  background-color: #fff;
  text-align: left;
}

.side-content {
  position: -webkit-sticky;
  position: sticky;
  top: 100px;
  border-top: 1px solid #eaeaea;
  border-left: 1px solid #eaeaea;
  border-right: 1px solid #eaeaea;
}

.category {
  padding: 16px;
  text-transform: capitalize;
  cursor: pointer;
  border-bottom: 1px solid #eaeaea;
  transition: all fade 0.1s;
}
.category:hover {
  background-color: #b4afaf;
  opacity: 0.5;
}

.tagged {
  background-color: #fa4a5b;
  color: #fff;
}

.count-res {
  font-size: 12px;
  margin-top: 8px;
  opacity: 0.6;
}

.collecton-name {
  font-size: 24px;
  text-align: left;
  text-transform: capitalize;
  margin: 0 32px;
  font-weight: 500;
}

.card-collection {
  display: -webkit-flex;
  display: flex;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
  max-width: 100%;
  margin: 8px;
}

.collection-name {
  font-size: 24px;
  text-align: left;
  text-transform: capitalize;
  margin: 0 32px;
  font-weight: 500;
}
.card-wrap {
  width: calc(100% - 36px);
  height: calc(100% - 81px);
  position: absolute;
  top: 16px;
  left: 16px;
  background-color: #fff;
  display: -webkit-flex;
  display: flex;
  -webkit-align-items: center;
  align-items: center;
  -webkit-justify-content: center;
  justify-content: center;
  font-size: 32px;
  color: #fa4a5b;
  border: 2px solid #fa4a5b;
  cursor: pointer;
  opacity: 0.9;
  transition: all 0.3s ease-in-out;
}
.n-display {
  display: none;
}

</style>
