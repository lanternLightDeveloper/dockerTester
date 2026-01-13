<script lang="ts">
	// 1. Types remain the same
	type Task = {
		id: string;
		title: string;
		done: boolean;
	};

	type Note = {
		id: string;
		content: string;
		sourceUrl: string;
	};

	type Section = {
		id: string;
		title: string;
		description: string;
		tasks: Task[];
		notes: Note[];
	};

	const STORAGE_KEY = 'home_server_learning_plan_v2';

	const defaultSections: Section[] = [
		{
			id: 'big-picture',
			title: '1. Big picture & internet vocabulary',
			description: 'Understand what you are building before touching hardware.',
			tasks: [
				{ id: 'server-vs-laptop', title: 'Server vs normal laptop', done: false },
				{ id: 'self-hosting-meaning', title: 'What self-hosting actually means', done: false },
				{ id: 'ip-addresses', title: 'IP addresses (local vs public)', done: false },
				{ id: 'domains-dns', title: 'Domain names & DNS (conceptual)', done: false },
				{ id: 'ports-http-https', title: 'Ports, HTTP, HTTPS (conceptual)', done: false }
			],
			notes: []
		},
		{
			id: 'linux-basics',
			title: '2. Linux & Ubuntu fundamentals',
			description: 'Become comfortable operating the server locally.',
			tasks: [
				{ id: 'install-ubuntu', title: 'Install Ubuntu on the old laptop', done: false },
				{
					id: 'terminal-basics',
					title: 'Terminal basics: ls, cd, cp, mv, rm, cat, nano',
					done: false
				},
				{ id: 'users-sudo', title: 'Users, sudo, root vs normal user', done: false },
				{ id: 'apt-install', title: 'Install software with apt', done: false },
				{ id: 'system-resources', title: 'Check CPU, RAM, disk usage', done: false }
			],
			notes: []
		},
		{
			id: 'networking-local',
			title: '3. Networking fundamentals (local)',
			description: 'Stabilize the server on your home network.',
			tasks: [
				{ id: 'local-vs-public-ip', title: 'Local IP vs public IP', done: false },
				{ id: 'router-nat', title: 'Router, NAT, and LAN basics', done: false },
				{ id: 'static-lan-ip', title: 'Assign a static LAN IP to the server', done: false },
				{ id: 'os-ports', title: 'Understand ports at the OS level', done: false }
			],
			notes: []
		},
		{
			id: 'docker-basics',
			title: '4. Docker fundamentals',
			description: 'Run applications in a repeatable, isolated way.',
			tasks: [
				{ id: 'install-docker', title: 'Install Docker and run hello-world', done: false },
				{ id: 'images-containers', title: 'Images vs containers', done: false },
				{
					id: 'docker-cli',
					title: 'Use docker run / ps / logs / stop / rm',
					done: false
				},
				{ id: 'docker-volumes', title: 'Use volumes for persistent data', done: false },
				{ id: 'docker-networks', title: 'Basic Docker networking concepts', done: false }
			],
			notes: []
		},
		{
			id: 'reverse-proxy',
			title: '5. Reverse proxy concepts',
			description: 'Understand traffic routing before adding security.',
			tasks: [
				{ id: 'proxy-what', title: 'What a reverse proxy is and why it exists', done: false },
				{
					id: 'proxy-choice',
					title: 'Choose a proxy (Caddy, Traefik, or Nginx)',
					done: false
				},
				{
					id: 'proxy-routing',
					title: 'Route a hostname to a container',
					done: false
				},
				{ id: 'ports-80-443', title: 'Understand ports 80 vs 443 in practice', done: false }
			],
			notes: []
		},
		{
			id: 'public-access',
			title: '6. Public reachability & DNS',
			description: 'Expose the server to the internet in a controlled way.',
			tasks: [
				{
					id: 'access-method',
					title: 'Choose port forwarding or a secure tunnel',
					done: false
				},
				{
					id: 'router-or-tunnel',
					title: 'Configure router forwarding or tunnel provider',
					done: false
				},
				{ id: 'get-domain', title: 'Purchase or select a domain', done: false },
				{
					id: 'dns-records',
					title: 'Point DNS (A/AAAA) to IP or tunnel endpoint',
					done: false
				},
				{
					id: 'external-test',
					title: 'Confirm basic external reachability',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'https-tls',
			title: '7. HTTPS & TLS',
			description: 'Secure all public traffic correctly.',
			tasks: [
				{ id: 'lets-encrypt', title: 'Obtain Let’s Encrypt certificates', done: false },
				{ id: 'enable-https', title: 'Enable HTTPS on the reverse proxy', done: false },
				{ id: 'force-https', title: 'Force HTTP → HTTPS redirects', done: false },
				{ id: 'auto-renew', title: 'Verify certificate auto-renewal', done: false },
				{
					id: 'tls-understanding',
					title: 'Understand TLS chains and renewals',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'coolify',
			title: '8. Coolify & app deployment',
			description: 'Deploy and manage applications using Coolify.',
			tasks: [
				{ id: 'install-coolify', title: 'Install Coolify', done: false },
				{
					id: 'coolify-dashboard',
					title: 'Explore the Coolify dashboard',
					done: false
				},
				{
					id: 'deploy-static',
					title: 'Deploy a simple static site',
					done: false
				},
				{ id: 'connect-git', title: 'Connect a Git repository', done: false },
				{
					id: 'domain-binding',
					title: 'Bind domains via Coolify proxy',
					done: false
				},
				{
					id: 'env-vars',
					title: 'Use environment variables and secrets',
					done: false
				},
				{ id: 'rollout', title: 'Update and roll out a new version', done: false }
			],
			notes: []
		},
		{
			id: 'security',
			title: '9. Server security & hardening',
			description: 'Reduce attack surface before going further.',
			tasks: [
				{ id: 'non-root-user', title: 'Create and use a non-root user', done: false },
				{ id: 'ssh-keys', title: 'Use SSH keys only', done: false },
				{ id: 'ssh-port', title: 'Change SSH port (optional)', done: false },
				{ id: 'enable-ufw', title: 'Enable UFW firewall', done: false },
				{
					id: 'ufw-rules',
					title: 'Allow only SSH, HTTP, HTTPS',
					done: false
				},
				{
					id: 'security-updates',
					title: 'Enable unattended security updates',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'docker-production',
			title: '10. Docker for production',
			description: 'Understand what Coolify abstracts away.',
			tasks: [
				{ id: 'dockerfile', title: 'Write a Dockerfile', done: false },
				{
					id: 'multi-stage',
					title: 'Use multi-stage production builds',
					done: false
				},
				{
					id: 'docker-env-vars',
					title: 'Environment variables in Docker',
					done: false
				},
				{
					id: 'volumes-vs-binds',
					title: 'Volumes vs bind mounts',
					done: false
				},
				{
					id: 'manual-run',
					title: 'Run containers without Coolify',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'backups',
			title: '11. Persistence & backups',
			description: 'Ensure the system can be rebuilt safely.',
			tasks: [
				{
					id: 'identify-state',
					title: 'Identify all stateful data',
					done: false
				},
				{
					id: 'backup-destination',
					title: 'Choose a backup destination',
					done: false
				},
				{
					id: 'automated-backups',
					title: 'Set up automated backups',
					done: false
				},
				{
					id: 'restore-test',
					title: 'Test restoring from backup',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'observability',
			title: '12. Observability & monitoring',
			description: 'Know when and why things break.',
			tasks: [
				{
					id: 'logs',
					title: 'Access Docker and Coolify logs',
					done: false
				},
				{
					id: 'journalctl',
					title: 'Read system logs with journalctl',
					done: false
				},
				{
					id: 'uptime-kuma',
					title: 'Deploy Uptime Kuma',
					done: false
				},
				{
					id: 'alerts',
					title: 'Set up disk and memory alerts',
					done: false
				}
			],
			notes: []
		},
		{
			id: 'failure-drills',
			title: '13. Failure drills & resilience',
			description: 'Practice recovery so outages do not cause panic.',
			tasks: [
				{
					id: 'simulate-outage',
					title: 'Simulate an outage and recover',
					done: false
				},
				{
					id: 'break-dns',
					title: 'Break DNS and fix it',
					done: false
				},
				{
					id: 'expired-cert',
					title: 'Simulate an expired TLS certificate',
					done: false
				},
				{
					id: 'disk-full',
					title: 'Fill disk space and observe behavior',
					done: false
				}
			],
			notes: []
		}
	];

	// Initial load helper
	function getInitialData(): Section[] {
		if (typeof localStorage === 'undefined') return defaultSections;

		const stored = localStorage.getItem(STORAGE_KEY);
		if (!stored) return defaultSections;

		try {
			const parsed = JSON.parse(stored);
			if (!Array.isArray(parsed)) return defaultSections;
			return normalizeSections(parsed);
		} catch {
			return defaultSections;
		}
	}

	// 2. State Runes
	let sections = $state<Section[]>(getInitialData());
	let selectedSectionId = $state(sections[0]?.id ?? '');
	let newTaskTitle = $state('');

	// 3. Derived Runes (Replacement for $:)
	let selectedSection = $derived(sections.find((s) => s.id === selectedSectionId) ?? sections[0]);

	let totalTasks = $derived(sections.reduce((sum, s) => sum + s.tasks.length, 0));

	let completedTasks = $derived(
		sections.reduce((sum, s) => sum + s.tasks.filter((t) => t.done).length, 0)
	);

	let progressPercent = $derived(
		totalTasks === 0 ? 0 : Math.round((completedTasks / totalTasks) * 100)
	);

	// 4. Effect Rune (Replacement for $: saveToStorage)
	$effect(() => {
		localStorage.setItem(STORAGE_KEY, JSON.stringify(sections));
	});

	// 5. Logic functions
	function toggleTask(task: Task) {
		// In Svelte 5, we can mutate directly!
		task.done = !task.done;
	}

	function addTask(sectionId: string) {
		const title = newTaskTitle.trim();
		if (!title) return;

		const section = sections.find((s) => s.id === sectionId);
		if (section) {
			section.tasks.push({
				id: `${sectionId}-${Date.now()}`,
				title,
				done: false
			});
			newTaskTitle = '';
		}
	}

	function removeTask(section: Section, taskId: string) {
		section.tasks = section.tasks.filter((t) => t.id !== taskId);
	}

	function resetProgress() {
		if (!confirm('Reset all progress and custom tasks?')) return;
		sections = JSON.parse(JSON.stringify(defaultSections)); // Deep copy
		selectedSectionId = sections[0].id;
	}

	let newNoteContent = $state('');
	let newNoteSource = $state('');

	function addNote(section: Section) {
		const content = newNoteContent.trim();
		if (!content) return;

		section.notes.push({
			id: `note-${Date.now()}`,
			content,
			sourceUrl: newNoteSource.trim()
		});

		newNoteContent = '';
		newNoteSource = '';
	}

	function removeNote(section: Section, noteId: string) {
		section.notes = section.notes.filter((n) => n.id !== noteId);
	}
</script>

<div class="app">
	<header>
		<h1>Home Server Learning Plan</h1>
		<p class="subtitle">Work through the phases, check things off, and add your own tasks.</p>
	</header>

	<main>
		<aside class="sidebar">
			{#each sections as section (section.id)}
				<button
					class="section-button"
					class:active={section.id === selectedSectionId}
					onclick={() => (selectedSectionId = section.id)}
				>
					<span>{section.title}</span>
					<small>
						{section.tasks.filter((t) => t.done).length}/{section.tasks.length}
					</small>
				</button>
			{/each}

			<div class="progress-container">
				<div class="progress-label">
					<span>Overall progress</span>
					<span>{completedTasks}/{totalTasks} ({progressPercent}%)</span>
				</div>
				<div class="progress-bar-track">
					<div class="progress-bar-fill" style:width="{progressPercent}%"></div>
				</div>
			</div>

			<button class="reset-button" onclick={resetProgress}> Reset all progress </button>
		</aside>

		{#if selectedSection}
			<section class="section-detail">
				<h2>{selectedSection.title}</h2>
				<p>{selectedSection.description}</p>

				<div class="tasks">
					{#each selectedSection.tasks as task (task.id)}
						<div class="task" class:done={task.done}>
							<label>
								<input type="checkbox" checked={task.done} onchange={() => toggleTask(task)} />
								<span class="task-title" class:done={task.done}>
									{task.title}
								</span>
							</label>
							<button
								class="task-remove"
								onclick={() => removeTask(selectedSection!, task.id)}
								title="Remove task"
							>
								✕
							</button>
						</div>
					{/each}
				</div>

				<div class="add-task">
					<input
						type="text"
						placeholder="Add your own task to this section..."
						bind:value={newTaskTitle}
						onkeydown={(e) => e.key === 'Enter' && addTask(selectedSection!.id)}
					/>
					<button onclick={() => addTask(selectedSection!.id)}>Add</button>
				</div>

				<div class="hint">
					Tip: As you learn, rewrite tasks in your own words. When a line feels “obvious,” mark it
					done.
				</div>
			</section>

			<div class="notes-print-area">
				<div class="notes">
					<h3>Notes</h3>

					{#if selectedSection.notes.length === 0}
						<p class="empty-notes">No notes yet for this section.</p>
					{/if}

					{#each selectedSection.notes as note (note.id)}
						<div class="note">
							<p class="note-content">{note.content}</p>

							{#if note.sourceUrl}
								<a
									class="note-source"
									href={note.sourceUrl}
									target="_blank"
									rel="noopener noreferrer"
								>
									Source - {note.sourceUrl}
								</a>
							{/if}

							<button class="note-remove" onclick={() => removeNote(selectedSection!, note.id)}>
								Remove
							</button>
						</div>
					{/each}

					<div class="add-note">
						<textarea placeholder="Write your note here…" bind:value={newNoteContent}></textarea>

						<input
							type="url"
							placeholder="Optional source link (article, docs, video)"
							bind:value={newNoteSource}
						/>

						<button onclick={() => addNote(selectedSection!)}> Add note </button>
						<button class="print-notes" onclick={() => window.print()}> Print Notes </button>
					</div>
				</div>
			</div>
		{/if}
	</main>
</div>

<style>
	:global(body) {
		margin: 0;
		font-family:
			system-ui,
			-apple-system,
			BlinkMacSystemFont,
			'Segoe UI',
			sans-serif;
		background: #081605;
		color: #e5e7eb;
	}

	.app {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
	}

	header {
		padding: 1.5rem 1.75rem;
		border-bottom: 1px solid rgba(148, 184, 151, 0.2);
		background: radial-gradient(circle at top left, #112a0f, #021702);
	}

	.subtitle {
		font-size: 0.9rem;
		color: #9ca3af;
	}

	main {
		display: grid;
		grid-template-columns: 260px 1fr;
		gap: 1rem;
		padding: 1rem 1.75rem 1.75rem;
	}

	@media (max-width: 800px) {
		main {
			grid-template-columns: 1fr;
		}
	}

	.sidebar {
		border-right: 1px solid rgba(148, 184, 153, 0.2);
		padding-right: 1rem;
	}

	@media (max-width: 800px) {
		.sidebar {
			border-right: none;
			border-bottom: 1px solid rgba(148, 184, 151, 0.2);
			padding-bottom: 1rem;
			margin-bottom: 1rem;
		}
	}

	.section-button {
		width: 100%;
		text-align: left;
		padding: 0.5rem 0.6rem;
		border-radius: 0.4rem;
		border: none;
		cursor: pointer;
		margin-bottom: 0.25rem;
		background: transparent;
		color: #e5e7eb;
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 0.9rem;
	}

	.section-button:hover {
		background: rgba(30, 175, 37, 0.4);
	}

	.section-button.active {
		background: linear-gradient(to right, #1dd826, #46e569);
	}

	.section-button small {
		font-size: 0.75rem;
		color: #ad9caf;
	}

	.section-button.active small {
		color: #ebe5eb;
	}

	.progress-container {
		margin-top: 0.75rem;
	}

	.progress-label {
		display: flex;
		justify-content: space-between;
		font-size: 0.75rem;
		color: #ae9caf;
		margin-bottom: 0.25rem;
	}

	.progress-bar-track {
		width: 100%;
		height: 6px;
		border-radius: 999px;
		background: rgba(31, 55, 37, 0.9);
		overflow: hidden;
	}

	.progress-bar-fill {
		height: 100%;
		border-radius: 999px;
		background: linear-gradient(to right, #22c55e, #a3e635);
		transition: width 150ms ease-out;
	}

	.reset-button {
		margin-top: 0.75rem;
		font-size: 0.75rem;
		color: #f97373;
		background: transparent;
		border: 1px solid rgba(248, 113, 113, 0.5);
		padding: 0.25rem 0.5rem;
		border-radius: 0.35rem;
		cursor: pointer;
	}

	.reset-button:hover {
		background: rgba(248, 113, 113, 0.1);
	}

	.section-detail {
		padding-left: 0.5rem;
	}

	.section-detail h2 {
		font-size: 1.1rem;
		margin: 0 0 0.25rem;
	}

	.section-detail p {
		font-size: 0.9rem;
		color: #ad9caf;
		margin: 0 0 0.75rem;
	}

	.note {
		width: 80vw;
	}

	.add-note {
		margin-top: 1.5rem;
	}

	.tasks,
	.notes {
		margin-top: 0.5rem;
	}

	.task,
	.note {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.45rem 0.5rem;
		border-radius: 0.35rem;
		background: rgba(15, 42, 19, 0.8);
		border: 1px solid rgba(51, 85, 51, 0.8);
		margin-bottom: 0.3rem;
	}

	.task.done {
		opacity: 0.7;
		background: rgba(22, 163, 74, 0.08);
		border-color: rgba(34, 197, 94, 0.5);
	}

	.task label {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		flex: 1;
		cursor: pointer;
		font-size: 0.9rem;
	}

	.task-title {
		flex: 1;
	}

	.task-title.done {
		text-decoration: line-through;
		color: #6d806b;
	}

	.task input[type='checkbox'] {
		width: 1rem;
		height: 1rem;
		accent-color: #22c55e;
	}

	.task-remove {
		border: none;
		background: transparent;
		color: #7e6b80;
		cursor: pointer;
		font-size: 0.85rem;
	}

	.task-remove:hover {
		color: #f97373;
	}

	.add-task {
		margin-top: 0.5rem;
		display: flex;
		gap: 0.4rem;
	}

	.add-task input,
	.add-note textarea,
	.add-note input {
		flex: 1;
		padding: 0.35rem 0.5rem;
		border-radius: 0.35rem;
		border: 1px solid rgba(55, 81, 57, 0.9);
		background: rgba(15, 42, 15, 0.9);
		color: #e5e7eb;
		font-size: 0.85rem;
	}

	.add-task button,
	.add-note button {
		padding: 0.35rem 0.6rem;
		border-radius: 0.35rem;
		border: none;
		background: #22c55e;
		color: #022c22;
		font-size: 0.85rem;
		cursor: pointer;
		font-weight: 500;
	}

	.add-task button:hover {
		background: #16a34a;
	}

	.hint {
		margin-top: 0.75rem;
		font-size: 0.8rem;
		color: #7d6b80;
	}

	@media print {
		main *,
		header,
		.hidden {
			visibility: hidden;
		}

		.notes-print-area,
		.notes-print-area * {
			visibility: visible;
		}

		.notes-print-area {
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			padding: 1rem;
		}

		/* Optional: hide remove buttons in print */
		.note-remove {
			display: none !important;
		}
	}
</style>
