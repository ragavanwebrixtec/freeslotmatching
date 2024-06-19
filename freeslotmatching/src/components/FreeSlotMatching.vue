<template>
  <div class="free-slot-matching">
    <div>
      <label for="slot-number">Number of Slots (N):</label>
      <input
        type="number"
        id="slot-number"
        v-model.number="numSlots"
        @input="initializeData"
        min="1"
        max="5"
      />
    </div>
    <div>
      <label for="member-number">Number of Members (X):</label>
      <input
        type="number"
        id="member-number"
        v-model.number="numMembers"
        @input="initializeData"
        min="1"
        max="10"
      />
    </div>

    <div class="background">
      <div class="slots">
        <div v-for="slot in freeSlots" :key="slot.id" class="slot">
          <div
            v-for="member in members"
            :key="member.id"
            :class="getSlotClass(slot.id, member.id)"
            class="slot-box"
            @click="toggleAvailability(slot.id, member.id)"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";

export default {
  name: "FreeSlotMatching",
  setup() {
    const state = reactive({
      numSlots: 3,
      numMembers: 5,
      freeSlots: [],
      members: [],
      availability: {},
    });

    const initializeData = () => {
      // Initialize freeSlots based on numSlots
      state.freeSlots = Array.from({ length: state.numSlots }, (v, i) => ({
        id: i + 1,
        name: `Slot ${i + 1}`,
      }));

      // Initialize members based on numMembers
      state.members = Array.from({ length: state.numMembers }, (v, i) => ({
        id: i + 1,
        name: `Member ${i + 1}`,
      }));

      // Initialize availability data structure
      state.availability = {};
      for (let i = 1; i <= state.numSlots; i++) {
        state.availability[i] = {};
        for (let j = 1; j <= state.numMembers; j++) {
          state.availability[i][j] = "unknown";
        }
      }
    };

    const getSlotClass = (slotId, memberId) => {
      const status = state.availability[slotId]?.[memberId];
      if (status === "available") return "available";
      if (status === "busy") return "busy";
      return "unknown";
    };

    const toggleAvailability = (slotId, memberId) => {
      const currentStatus = state.availability[slotId][memberId];
      if (currentStatus === "unknown") {
        state.availability[slotId][memberId] = "available";
      } else if (currentStatus === "available") {
        state.availability[slotId][memberId] = "busy";
      } else if (currentStatus === "busy") {
        state.availability[slotId][memberId] = "unknown";
      }
    };

    // Initialize data on component mount
    initializeData();

    return {
      ...toRefs(state),
      initializeData,
      getSlotClass,
      toggleAvailability,
    };
  },
};
</script>

<style scoped>
.free-slot-matching {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

label {
  margin: 5px 0;
}

input {
  margin: 0 0 15px;
  padding: 5px;
  width: 100px;
}

.slots {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.background {
  background-color: #000;
  margin: 5px;
  padding: 5px 10px;
  border-radius: 10px;
}

.slot {
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
}

.slot-box {
  width: 50px;
  height: 50px;
  margin: 5px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #000;
  color: #fff;
  font-weight: bold;
  transition: background-color 0.3s ease;
  cursor: pointer;
  border-radius: 10px;
}

.slot-box.unknown {
  background-color: grey;
}

.slot-box.available {
  background-color: green;
}

.slot-box.busy {
  background-color: red;
}
</style>
