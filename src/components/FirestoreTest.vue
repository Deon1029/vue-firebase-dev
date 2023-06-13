<template>
  <div style="display: flex; flex-direction: column; align-items: center;">
    <div>{{ msg }}</div>
    <div>
      <ul>
        <li v-for="(item, idx) in result" :key="idx">
          {{ item.unitId }}<br/>
          {{ item.campusCode }}<br/>
          위도 : {{ item.location.latitude }}<br/>
          경도 : {{ item.location.longitude }}<br/>
          <br/>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { initializeApp } from "firebase/app";
import { getFirestore, collection, query, where, onSnapshot } from "firebase/firestore";

export default {
  name: 'FirestoreTest',
  props: {
    msg: String
  },
  data() {
    return {
      result: []
    }
  },
  created() {
    const firebaseConfig = {
      apiKey: "AIzaSyCnRn5OjrdiMVS3UgxoOGFYjf9XVE6dhg4",
      authDomain: "deon-vue-app-test.firebaseapp.com",
      projectId: "deon-vue-app-test",
      storageBucket: "deon-vue-app-test.appspot.com",
      messagingSenderId: "523527133558",
      appId: "1:523527133558:web:17517cd22d937a7fac0257",
      measurementId: "G-27T6DLJZVK"
    };
    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);

    const q = query(collection(db, "DeliverUnitLatLng"), where("campusCode", "==", "D1"))
    // const q = query(collection(db, "TestArticles"))
    // eslint-disable-next-line
    const unsubscribe = onSnapshot(q, (querySnapshot) => {
      const units = [];
      querySnapshot.forEach((doc) => {
          units.push(doc.data());
      });
      console.log(units)
      // console.log(units.join(", "));
      this.result = units;
    });
  }
}

</script>

<style>

</style>