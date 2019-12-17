<template>
  <q-page class="row">
    <div class="col-md-5 col-xs-12 q-px-md">

      <div class="q-pa-md rounded-borders overflow-y-auto">

          <div id="photo" class="q-mb-xl">
            <div class="text-h6">Photo</div>
            <span class="text-body2">Upload your photo</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-uploader />
          </div>

          <div id="name" class="q-mb-xl">
            <div class="text-h6">Name</div>
            <span class="text-body2">Use your first and last name. No nicknames</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm overflow-hidden">
              <div class="row">
                <div class="col-6 q-pr-xs">
                  <q-input
                    filled
                    v-model="firstName"
                    hint="First Name"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type something']"
                  />
                </div>
                <div class="col-6 q-pl-xs">
                  <q-input
                    filled
                    v-model="lastName"
                    hint="Last Name"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type something']"
                  />
                </div>
              </div>
            </q-form>
          </div>

          <div id="subtitle" class="q-mb-xl">
            <div class="text-h6">Subtitle</div>
            <span class="text-body2">Two words what you want to do</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm overflow-hidden">
              <q-input
                filled
                v-model="subtitle"
                hint="Subtitle"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
            </q-form>
          </div>

          <div id="contacts" class="q-mb-xl">
            <div class="text-h6">Contacts</div>
            <span class="text-body2">Add your phone number and email</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm overflow-hidden">
              <div class="row">
                <div class="col-6 q-pr-xs">
                  <q-input
                    filled
                    v-model="phone"
                    hint="paul.keen@jetthoughts.com"
                    type="email"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type something']"
                  />
                </div>
                <div class="col-6 q-pl-xs">
                  <q-input
                    filled
                    v-model="email"
                    hint="paul.keen@jetthoughts.com"
                    type="email"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type something']"
                  />
                </div>
              </div>
            </q-form>
          </div>

          <div id="experience" class="q-mb-xl relative-position q-pb-lg">
            <div class="text-h6">Experience</div>
            <span class="text-body2">Add at least 3 jobs with 3 highlights each</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm q-mb-xl relative-position q-pt-md" v-for="index in experiences.length" :key="index">

              <span class="text-overline">Experience {{ index }}</span>
              <q-btn
                round
                dense
                class="absolute-top-right bg-white"
                icon="delete_outline"
                @click="removeExperience(index)"
              />

              <q-input
                filled
                v-model="experiences[index - 1].company"
                hint="Company or Project Name"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
              <q-input
                filled
                v-model="experiences[index - 1].positionTitle"
                hint="Position Title"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
              <div class="row">
                <div class="col-6 q-pr-xs">
                  <q-input
                    v-model="experiences[index - 1].startExpienceDate"
                    filled type="date"
                    hint="Start Date"
                  />
                </div>
                <div class="col-6 q-pl-xs">
                  <q-input
                    v-model="experiences[index - 1].endExpienceDate"
                    filled type="date"
                    hint="End Date"
                  />
                </div>
              </div>

              <q-input
                bottom-slots
                filled
                v-model="experiences[index - 1].highlight"
                hint="New Highlights"
                lazy-rules
                type="textarea"
                style="height: 95px;"
              >
                <template v-slot:hint>Skill (E.g. Design, Management, etc.)</template>
                <template v-slot:append>
                  <q-btn
                    round
                    dense
                    icon="add"
                    class="bg-white absolute-bottom-right"
                    style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                    @click="experiences[index - 1].highlight && addHighlights(experiences[index - 1].highlight, index)"
                  />
                </template>
              </q-input>

              <div
                v-for="highlight in experiences[index - 1].highlights"
                :key="highlight"
                class="text-body2 q-mb-lg bg-grey-3 q-pa-md relative-position"
              >
                {{ highlight }}
                <q-btn
                  round
                  dense
                  icon="remove"
                  class="bg-white block absolute-bottom-right"
                  style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                  @click="removeHighlights(highlight, index)"
                />
              </div>

            </q-form>
            <q-btn
              round
              dense
              class="absolute-bottom-right bg-white"
              icon="post_add"
              @click="addExperience"
            />
          </div>

          <div id="education" class="q-mb-xl q-pb-lg relative-position">
            <div class="text-h6">Education</div>
            <span class="text-body2">List all your relevant education</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm q-mb-md q-pt-md relative-position" v-for="index in educations.length" :key="index">

              <span class="text-overline">Education {{ index }}</span>
              <q-btn
                round
                dense
                class="absolute-top-right bg-white"
                icon="delete_outline"
                @click="removeEducation(index)"
              />

              <q-input
                filled
                v-model="educations[index - 1].school"
                hint="School"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
              <q-input
                filled
                v-model="educations[index - 1].degree"
                hint="Degree"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
              <q-input
                v-model="educations[index - 1].educationDate"
                filled type="date"
                hint="Graduation Date"
              />
              <q-input
                filled
                v-model="educations[index - 1].fieldOfStudy"
                hint="Field of Study"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
              <q-input
                filled
                v-model="educations[index - 1].achievements"
                hint="Achievements"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
            </q-form>
            <q-btn
              round
              dense
              class="absolute-bottom-right bg-white"
              icon="post_add"
              @click="addEducation"
            />
          </div>

          <div id="skills" class="q-mb-xl">
            <div class="text-h6">Skills</div>
            <span class="text-body2">List both your hard and soft skills, putting the most relevant skills at the top of the list</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm q-mb-sm relative-position">
              <q-input bottom-slots filled v-model="skill">
                <template v-slot:hint>Skill (E.g. Design, Management, etc.)</template>
                <template v-slot:append>
                  <q-btn
                    round
                    dense
                    icon="add"
                    @click="skill && addSkill(skill)"
                    class="bg-white block absolute-bottom-right"
                    style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                  />
                </template>
              </q-input>
            </q-form>

            <div
              v-for="skill in skills"
              :key="skill"
              class="text-body2 q-mb-lg bg-grey-3 q-pa-md relative-position"
            >
              {{ skill }}
              <q-btn
                round
                dense
                icon="remove"
                class="bg-white block absolute-bottom-right"
                style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                @click="removeSkill(skill)"
              />
            </div>

          </div>

          <div id="languages" class="q-mb-xl">
            <div class="text-h6">Languages</div>
            <span class="text-body2">List all the languages you know</span>
            <q-separator class="q-mt-md q-mb-md" />

            <q-form class="q-gutter-sm q-mb-sm relative-position">
              <q-input bottom-slots filled v-model="language">
                <template v-slot:hint>Language (E.g. English)</template>
                <template v-slot:append>
                  <q-btn
                    round
                    dense
                    icon="add"
                    @click="language && addLanguage(language)"
                    class="bg-white block absolute-bottom-right"
                    style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                  />
                </template>
              </q-input>
            </q-form>

            <div
              v-for="language in languages"
              :key="language"
              class="text-body2 q-mb-lg bg-grey-3 q-pa-md relative-position"
            >
              {{ language }}
              <q-btn
                round
                dense
                icon="remove"
                class="bg-white block absolute-bottom-right"
                style="margin-bottom: -17px; margin-right: -17px; z-index: 9"
                @click="removeLanguage(language)"
              />
            </div>

          </div>

          <div id="summary" class="q-mb-xl">
            <div class="text-h6">Summary</div>
            <span class="text-body2">Write highlights about you</span>
            <q-separator class="q-mt-md q-mb-md" />
            <q-form class="q-gutter-sm overflow-hidden">
              <q-input
                filled
                v-model="summary"
                hint="Your Professional Summary"
                type="textarea"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']"
              />
            </q-form>
          </div>

      </div>

    </div>
    <div class="col-md-7 col-sm-12 q-px-md">
      <div style="max-width: 670px; margin: 0 auto;" class="overflow-y-auto q-pa-md">

          <div v-if="firstName || lastName || subtitle || phone || email" class="q-mb-xl overflow-hidden">
            <img
              class="float-left q-mr-md"
              width="200"
              v-if="imageSrc"
              :src="imageSrc"
              alt="Avatar"
            />
            <div class="overflow-hidden">
              <h4 class="q-my-sm text-h4">
                <span class="q-mr-sm">{{ firstName }}</span>
                <span>{{ lastName }}</span>
              </h4>
              <div class="text-subtitle1 q-mb-sm">{{ subtitle }}</div>
              <div>
                <span class="text-subtitle2 q-mr-sm">Email:</span>
                <span class="text-body2">{{ email }}</span>
              </div>
              <div>
                <span class="text-subtitle2 q-mr-sm">Phone:</span>
                <span class="text-body2">{{ phone }}</span>
              </div>
            </div>
          </div>

          <div v-if="experiences.length" class="q-mb-xl">
            <h6 class="text-h6 q-mt-none q-mb-xs">Experience:</h6>
            <q-separator class="q-mb-md q-pb-xs bg-secondary" />

            <div class="overflow-hidden q-mb-lg" v-for="experience in experiences" :key="experience">
              <div
                class="text-subtitle1 text-grey-7 float-left q-mr-md"
                style="min-width: 86px;"
              >
                {{ experience.startExpienceDate.substring(0, 4) }} - {{ experience.endExpienceDate.substring(0, 4) }}
              </div>

              <div class="overflow-hidden">
                <div class="text-subtitle2">{{ experience.positionTitle }}</div>
                <div class="text-subtitle1 text-grey-7">{{ experience.company }}</div>
                <div class="text-body2 q-mt-md" v-for="highlight in experience.highlights" :key="highlight">{{ highlight }}</div>
              </div>
            </div>
          </div>

          <div v-if="educations.length" class="q-mb-xl">
            <h6 class="text-h6 q-mt-none q-mb-xs">Education:</h6>
            <q-separator class="q-mb-md q-pb-xs bg-secondary" />

            <div class="overflow-hidden q-mb-lg" v-for="education in educations" :key="education">
              <div
                class="text-subtitle1 text-grey-7 float-left q-mr-md"
                style="min-width: 86px;"
              >
                {{ education.educationDate.substring(0, 4) }}
              </div>

              <div class="overflow-hidden">
                <div class="text-subtitle2">{{ education.fieldOfStudy }}</div>
                <div class="text-subtitle1 text-grey-7">{{ education.school }}</div>
                <div class="text-body2">{{ education.degree }}</div>
                <div class="text-body2">{{ education.achievements }}</div>
              </div>
            </div>

          </div>

          <div v-if="skills.length" class="q-mb-xl overflow-hidden">
            <h6 class="text-h6 q-mt-none q-mb-xs">Skills:</h6>
            <q-separator class="q-mb-md q-pb-xs bg-secondary" />

            <div
              v-for="skill in skills"
              :key="skill"
              class="text-body2"
            >
              {{ skill }}
            </div>
          </div>

          <div v-if="languages.length" class="q-mb-xl overflow-hidden">
            <h6 class="text-h6 q-mt-none q-mb-xs">Languages:</h6>
            <q-separator class="q-mb-md q-pb-xs bg-secondary" />

            <div
              v-for="language in languages"
              :key="language"
              class="text-body2"
            >
              {{ language }}
            </div>
          </div>

          <div v-if="summary" class="q-mb-xl overflow-hidden">
            <h6 class="text-h6 q-mt-none q-mb-xs">Summary:</h6>
            <q-separator class="q-mb-md q-pb-xs bg-secondary" />

            <div class="text-body2">{{ summary }}</div>
          </div>

      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  methods: {
    onFileSelected (event) {
      this.$data.imageSrc = URL.createObjectURL(event.target.files[0])
    },
    addSkill (skill) {
      this.$data.skills.push(skill)
      this.$data.skill = ''
    },
    removeSkill (skill) {
      this.$data.skills = this.$data.skills.filter(item => item !== skill)
    },
    addLanguage (language) {
      this.$data.languages.push(language)
      this.$data.language = ''
    },
    removeLanguage (language) {
      this.$data.languages = this.$data.languages.filter(item => item !== language)
    },
    addHighlights (highlight, index) {
      this.$data.experiences[index - 1].highlights.push(highlight)
      this.$data.experiences[index - 1].highlight = ''
    },
    removeHighlights (highlight, index) {
      this.$data.experiences[index - 1].highlights = this.$data.experiences[index - 1].highlights.filter(item => item !== highlight)
    },
    addExperience () {
      this.$data.experiences.push({ positionTitle: '', company: '', startExpienceDate: '', endExpienceDate: '', highlight: '', highlights: [] })
    },
    removeExperience (index) {
      this.$data.experiences = this.$data.experiences.splice(index - 1, 1)
    },
    addEducation () {
      this.$data.educations.push({ school: '', degree: '', educationDate: '', fieldOfStudy: '', achievements: '' })
    },
    removeEducation (index) {
      this.$data.educations = this.$data.educations.splice(index - 1, 1)
    }
  },

  data () {
    return {
      imageSrc: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTrnX_qVdYQsabmxytonuE3S7NVN9CTmOmC9UWREIfQg1lpZ25Z&s',
      panel: 'name',
      firstName: 'John',
      lastName: 'Wayne',
      subtitle: 'React Developer',
      phone: '+380501553318',
      email: 'john.wayne@email.com',
      experiences: [
        {
          positionTitle: 'Software engineer',
          company: 'Jetthoughts',
          startExpienceDate: '2015-01-15',
          endExpienceDate: '2019-05-01',
          highlight: '',
          highlights: ['In order to write an engaging, results-driven resume, you should state not only what you did, but what you achieved by what you did, framing it in terms of numbers.', 'In order to write an engaging, results-driven resume, you should state not only what you did, but what you achieved by what you did framing it in terms of numbers.']
        },
        {
          positionTitle: 'Software engineer',
          company: 'Jetthoughts',
          startExpienceDate: '2018-01-15',
          endExpienceDate: '2020-05-01',
          highlight: '',
          highlights: ['In order to write an engaging, results-driven resume, you should state not only what you did, but what you achieved by what you did, framing it in terms of numbers.']
        }
      ],
      educations: [
        {
          school: 'Lvivska Polinehnika',
          degree: 'Magister',
          educationDate: '2011-12-21',
          fieldOfStudy: 'Computer science',
          achievements: 'Academic achievement'
        },
        {
          school: 'Lvivska Polinehnika',
          degree: 'Magister',
          educationDate: '2005-12-21',
          fieldOfStudy: 'Computer science',
          achievements: 'Academic achievement'
        }
      ],
      skill: '',
      skills: ['React', 'React Native'],
      language: '',
      languages: ['Spanish'],
      summary: 'A resume summary statement is a short paragraph at the beginning of a resume that highlights a job seeker’s professional skills and experience. It gives hiring managers a glimpse into the job seeker’s expertise before diving into their resume. The goal of a summary statement is to demonstrate the job seeker’s unique value through their skills and accomplishments.'
    }
  }
}
</script>
