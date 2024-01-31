<template>
	<footer class="absolute bottom-0 left-0 right-0 flex text-center text-white">
		<div class="flex-1 p-1" style="background:#274684">
			{{ $slidev.configs.date || new Date().toLocaleDateString() }}
		</div>
		<div class="flex-1 p-1" style="background:#070c2c">
			{{ $slidev.configs.author || "Unknown author" }}
		</div>
		<div class="flex-1 p-1" style="background:#709fdc">
				<SlideCurrentNo /> /
				<SlidesTotal />
		</div>
	</footer>
</template>
