FROM --platform=linux/amd64 node:alpine As development

WORKDIR /usr/src/app

COPY tsconfig.json ./
COPY package*.json ./
COPY ./prisma ./prisma/
COPY . .

RUN npm install
RUN npm run build
RUN npx prisma generate


FROM --platform=linux/amd64 node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY tsconfig.json ./
COPY package*.json ./
COPY ./prisma ./prisma/

RUN npm install --only=production
RUN npx prisma generate
COPY . .

COPY --from=development /usr/src/app/dist ./dist
# COPY ./dist ./dist
# COPY ./apps/users/src/views ./dist/apps/users

# 
CMD ["node", "dist/apps/users/main"]