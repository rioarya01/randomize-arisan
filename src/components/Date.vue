<template>
    <div class="text-xl md:text-2xl lg:text-4xl font-bold text-secondary flex flex-col gap-1 lg:gap-4">
        <p>📅 <span class="date">{{ formattedDate }}</span></p>
        <p>🕛 <span class="time">{{ formattedTime }}</span></p>
    </div>
</template>

<script>
export default {
    data() {
        return {
        formattedDate: '', // Ini adalah data reactive untuk menampilkan tanggal
        formattedTime: '', // Ini adalah data reactive untuk menampilkan waktu
        };
    },
    mounted() {
        this.updateDateTime(); // Panggil fungsi untuk memperbarui tanggal dan waktu saat komponen dimount
    },
    methods: {
        updateDateTime() {
        let tw = new Date();
        let a = tw.getTimezoneOffset() === 0 ? tw.getTime() + (7 * 60 * 60 * 1000) : tw.getTime();
        tw.setTime(a);
        
        let tahun = tw.getFullYear();
        let hari = tw.getDay();
        let bulan = tw.getMonth();
        let tanggal = tw.getDate();
        let jam = tw.getHours();
        let menit = tw.getMinutes();
        
        const hariArray = ["Minggu,", "Senin,", "Selasa,", "Rabu,", "Kamis,", "Jum'at,", "Sabtu,"];
        const bulanArray = ["Januari", "Februari", "Maret", "April", "Mei", "Juni", "Juli", "Agustus", "September", "Oktober", "Nopember", "Desember"];
        
        this.formattedDate = hariArray[hari] + " " + tanggal + " " + bulanArray[bulan] + " " + tahun;
        this.formattedTime = ((jam < 10) ? "0" : "") + jam + ":" + ((menit < 10) ? "0" : "") + menit + " wita";
        },
    },
};
</script>